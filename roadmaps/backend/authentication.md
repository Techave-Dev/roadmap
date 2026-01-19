---
sidebar_position: 5
---

# Authentication & Security

Learn to secure your applications with authentication, authorization, and security best practices.

## Authentication vs Authorization

- **Authentication**: *Who are you?* - Verifying user identity
- **Authorization**: *What can you do?* - Checking user permissions

---

## Authentication Methods

### 1. Session-Based Authentication

Traditional approach using server-side sessions.

**How it works:**
1. User submits credentials (username/password)
2. Server validates and creates a session
3. Server stores session data (in-memory, database, Redis)
4. Server sends session ID to client via cookie
5. Client sends cookie with each request
6. Server validates session ID

**Example Flow:**
```javascript
// Login endpoint
app.post('/login', async (req, res) => {
  const { email, password } = req.body;
  
  // Validate credentials
  const user = await db.users.findOne({ email });
  const isValid = await bcrypt.compare(password, user.passwordHash);
  
  if (!isValid) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  // Create session
  req.session.userId = user.id;
  req.session.role = user.role;
  
  res.json({ message: 'Logged in successfully' });
});

// Protected route
app.get('/profile', requireAuth, async (req, res) => {
  const user = await db.users.findById(req.session.userId);
  res.json(user);
});
```

**Pros:**
- Simple to implement
- Session can be revoked instantly
- Server has full control

**Cons:**
- Server must store session data
- Harder to scale horizontally
- Not great for APIs consumed by mobile apps

---

### 2. Token-Based Authentication (JWT)

Modern approach using JSON Web Tokens.

**How it works:**
1. User submits credentials
2. Server validates and creates a JWT
3. Server signs JWT with secret key
4. Client stores token (localStorage, cookie)
5. Client sends token in Authorization header
6. Server verifies signature and extracts data

**JWT Structure:**
```
header.payload.signature

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjMiLCJyb2xlIjoidXNlciIsImlhdCI6MTYxNjIzOTAyMiwiZXhwIjoxNjE2MjQyNjIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

**JWT Payload Example:**
```json
{
  "userId": "123",
  "email": "john@example.com",
  "role": "user",
  "iat": 1616239022,  // Issued at
  "exp": 1616242622   // Expires at
}
```

**Example Implementation:**
```javascript
// Login endpoint
app.post('/login', async (req, res) => {
  const { email, password } = req.body;
  
  const user = await db.users.findOne({ email });
  const isValid = await bcrypt.compare(password, user.passwordHash);
  
  if (!isValid) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  // Create JWT
  const token = jwt.sign(
    { userId: user.id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '7d' }
  );
  
  res.json({ token });
});

// Middleware to verify JWT
const requireAuth = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1]; // "Bearer TOKEN"
  
  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

// Protected route
app.get('/profile', requireAuth, async (req, res) => {
  const user = await db.users.findById(req.user.userId);
  res.json(user);
});
```

**Pros:**
- Stateless (no server storage needed)
- Great for APIs and microservices
- Works well across domains
- Scalable

**Cons:**
- Cannot revoke tokens before expiry (use short expiry + refresh tokens)
- Token size (larger than session ID)
- Must protect secret key

---

### 3. OAuth 2.0

Delegated authorization protocol for third-party login.

**Common Flows:**

**Authorization Code Flow (most secure):**
```
1. User clicks "Login with Google"
2. Redirect to Google's authorization page
3. User approves
4. Google redirects back with authorization code
5. Exchange code for access token
6. Use token to fetch user data
```

**Example (Node.js with Passport.js):**
```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: '/auth/google/callback'
  },
  async (accessToken, refreshToken, profile, done) => {
    // Find or create user
    let user = await db.users.findOne({ googleId: profile.id });
    
    if (!user) {
      user = await db.users.create({
        googleId: profile.id,
        email: profile.emails[0].value,
        name: profile.displayName
      });
    }
    
    done(null, user);
  }
));

// Routes
app.get('/auth/google', passport.authenticate('google', {
  scope: ['profile', 'email']
}));

app.get('/auth/google/callback', 
  passport.authenticate('google', { failureRedirect: '/login' }),
  (req, res) => {
    // Successful authentication
    res.redirect('/dashboard');
  }
);
```

**Popular OAuth Providers:**
- Google
- GitHub
- Facebook
- Twitter/X
- Microsoft
- Apple

---

### 4. Refresh Tokens

Extend sessions securely with short-lived access tokens and long-lived refresh tokens.

**Flow:**
1. Login returns access token (short expiry, e.g., 15 min) + refresh token (long expiry, e.g., 7 days)
2. Client uses access token for API requests
3. When access token expires, use refresh token to get new access token
4. Refresh tokens stored securely (database, can be revoked)

**Example:**
```javascript
app.post('/login', async (req, res) => {
  // ... validate credentials ...
  
  const accessToken = jwt.sign(
    { userId: user.id },
    process.env.JWT_SECRET,
    { expiresIn: '15m' }
  );
  
  const refreshToken = jwt.sign(
    { userId: user.id },
    process.env.REFRESH_SECRET,
    { expiresIn: '7d' }
  );
  
  // Store refresh token in database
  await db.refreshTokens.create({
    userId: user.id,
    token: refreshToken,
    expiresAt: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)
  });
  
  res.json({ accessToken, refreshToken });
});

app.post('/refresh', async (req, res) => {
  const { refreshToken } = req.body;
  
  // Verify refresh token
  const decoded = jwt.verify(refreshToken, process.env.REFRESH_SECRET);
  
  // Check if token exists in database
  const storedToken = await db.refreshTokens.findOne({
    userId: decoded.userId,
    token: refreshToken
  });
  
  if (!storedToken) {
    return res.status(401).json({ error: 'Invalid refresh token' });
  }
  
  // Issue new access token
  const newAccessToken = jwt.sign(
    { userId: decoded.userId },
    process.env.JWT_SECRET,
    { expiresIn: '15m' }
  );
  
  res.json({ accessToken: newAccessToken });
});
```

---

## Password Security

### 1. Hashing Passwords

**NEVER** store passwords in plain text. Always hash them.

```javascript
const bcrypt = require('bcrypt');

// Hash password during registration
const saltRounds = 10;
const passwordHash = await bcrypt.hash(password, saltRounds);
await db.users.create({ email, passwordHash });

// Verify password during login
const user = await db.users.findOne({ email });
const isValid = await bcrypt.compare(password, user.passwordHash);
```

**Recommended Algorithms:**
- **bcrypt** - Industry standard
- **argon2** - Modern, more secure
- **scrypt** - Also good

**Never use:**
- MD5
- SHA-1
- Plain SHA-256 (too fast, vulnerable to brute force)

### 2. Password Requirements

```javascript
const validatePassword = (password) => {
  const minLength = 8;
  const hasUpperCase = /[A-Z]/.test(password);
  const hasLowerCase = /[a-z]/.test(password);
  const hasNumbers = /\d/.test(password);
  const hasSpecialChar = /[!@#$%^&*]/.test(password);
  
  return (
    password.length >= minLength &&
    hasUpperCase &&
    hasLowerCase &&
    hasNumbers &&
    hasSpecialChar
  );
};
```

### 3. Password Reset

```javascript
// Generate reset token
app.post('/forgot-password', async (req, res) => {
  const { email } = req.body;
  const user = await db.users.findOne({ email });
  
  if (!user) {
    // Don't reveal if email exists
    return res.json({ message: 'If email exists, reset link sent' });
  }
  
  // Generate secure random token
  const resetToken = crypto.randomBytes(32).toString('hex');
  const resetTokenHash = await bcrypt.hash(resetToken, 10);
  
  await db.users.update(user.id, {
    resetToken: resetTokenHash,
    resetTokenExpiry: Date.now() + 3600000 // 1 hour
  });
  
  // Send email with reset link
  await sendEmail({
    to: email,
    subject: 'Password Reset',
    body: `Reset your password: https://example.com/reset?token=${resetToken}`
  });
  
  res.json({ message: 'If email exists, reset link sent' });
});

// Reset password
app.post('/reset-password', async (req, res) => {
  const { token, newPassword } = req.body;
  
  const user = await db.users.findOne({
    resetTokenExpiry: { $gt: Date.now() }
  });
  
  if (!user) {
    return res.status(400).json({ error: 'Invalid or expired token' });
  }
  
  const isValid = await bcrypt.compare(token, user.resetToken);
  
  if (!isValid) {
    return res.status(400).json({ error: 'Invalid token' });
  }
  
  // Update password
  const newPasswordHash = await bcrypt.hash(newPassword, 10);
  await db.users.update(user.id, {
    passwordHash: newPasswordHash,
    resetToken: null,
    resetTokenExpiry: null
  });
  
  res.json({ message: 'Password reset successfully' });
});
```

---

## Authorization

### Role-Based Access Control (RBAC)

```javascript
// Middleware to check roles
const requireRole = (...allowedRoles) => {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Not authenticated' });
    }
    
    if (!allowedRoles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    
    next();
  };
};

// Usage
app.delete('/users/:id', requireAuth, requireRole('admin'), async (req, res) => {
  await db.users.delete(req.params.id);
  res.json({ message: 'User deleted' });
});
```

### Permission-Based Access Control

```javascript
// More granular than roles
const permissions = {
  'users:read': ['admin', 'user'],
  'users:write': ['admin'],
  'users:delete': ['admin'],
  'posts:create': ['admin', 'user']
};

const requirePermission = (permission) => {
  return (req, res, next) => {
    const allowedRoles = permissions[permission];
    
    if (!allowedRoles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    
    next();
  };
};
```

---

## Security Best Practices

### 1. HTTPS Only
Always use HTTPS in production. Encrypt all data in transit.

### 2. CORS Configuration
```javascript
const cors = require('cors');

app.use(cors({
  origin: ['https://yourdomain.com'],
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));
```

### 3. Rate Limiting
```javascript
const rateLimit = require('express-rate-limit');

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // 5 attempts
  message: 'Too many login attempts, please try again later'
});

app.post('/login', loginLimiter, async (req, res) => {
  // ... login logic ...
});
```

### 4. Input Validation
```javascript
const { body, validationResult } = require('express-validator');

app.post('/register',
  body('email').isEmail().normalizeEmail(),
  body('password').isLength({ min: 8 }),
  body('name').trim().escape(),
  async (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    
    // ... registration logic ...
  }
);
```

### 5. SQL Injection Prevention
```javascript
// ❌ VULNERABLE
const query = `SELECT * FROM users WHERE email = '${email}'`;

// ✅ SAFE (parameterized query)
const query = 'SELECT * FROM users WHERE email = ?';
const user = await db.query(query, [email]);

// ✅ SAFE (ORM)
const user = await db.users.findOne({ email });
```

### 6. XSS Prevention
```javascript
// Escape user input before rendering
const escapeHtml = (unsafe) => {
  return unsafe
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;")
    .replace(/'/g, "&#039;");
};

// Use Content Security Policy
app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    scriptSrc: ["'self'", "'unsafe-inline'"]
  }
}));
```

### 7. CSRF Protection
```javascript
const csrf = require('csurf');

app.use(csrf({ cookie: true }));

app.get('/form', (req, res) => {
  res.render('form', { csrfToken: req.csrfToken() });
});
```

### 8. Security Headers
```javascript
const helmet = require('helmet');

app.use(helmet()); // Sets various HTTP headers for security
```

---

## Multi-Factor Authentication (MFA)

Add extra layer of security with OTP (One-Time Password).

```javascript
const speakeasy = require('speakeasy');
const QRCode = require('qrcode');

// Generate secret for user
app.post('/mfa/setup', requireAuth, async (req, res) => {
  const secret = speakeasy.generateSecret({
    name: `YourApp (${req.user.email})`
  });
  
  await db.users.update(req.user.id, {
    mfaSecret: secret.base32,
    mfaEnabled: false
  });
  
  // Generate QR code
  const qrCodeUrl = await QRCode.toDataURL(secret.otpauth_url);
  
  res.json({ qrCode: qrCodeUrl, secret: secret.base32 });
});

// Verify and enable MFA
app.post('/mfa/verify', requireAuth, async (req, res) => {
  const { token } = req.body;
  const user = await db.users.findById(req.user.id);
  
  const verified = speakeasy.totp.verify({
    secret: user.mfaSecret,
    encoding: 'base32',
    token: token
  });
  
  if (verified) {
    await db.users.update(req.user.id, { mfaEnabled: true });
    res.json({ message: 'MFA enabled successfully' });
  } else {
    res.status(400).json({ error: 'Invalid token' });
  }
});
```

---

## Learning Path

1. **Start with basics**
   - Implement login/registration with sessions
   - Hash passwords with bcrypt
   - Add input validation

2. **Move to JWT**
   - Implement token-based auth
   - Add refresh tokens
   - Handle token expiration

3. **Add OAuth**
   - Integrate Google/GitHub login
   - Understand OAuth flows

4. **Implement authorization**
   - Role-based access control
   - Permission systems

5. **Security hardening**
   - Rate limiting
   - CSRF protection
   - Security headers

6. **Advanced topics**
   - Multi-factor authentication
   - SSO (Single Sign-On)
   - API keys for service-to-service auth

## Practice Projects

1. **User Authentication System** - Login, registration, password reset
2. **JWT API** - Token-based auth for REST API
3. **OAuth Integration** - Social login with Google/GitHub
4. **Admin Dashboard** - Role-based access control
5. **MFA Implementation** - Add 2FA to existing app

## Next Steps

- **Architecture**: Design secure, scalable systems
- **Deployment**: Secure production deployments
