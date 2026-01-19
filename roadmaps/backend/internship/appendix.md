# Appendix: Quick Reference

A collection of handy reference materials, templates, and commands you'll use throughout your internship.

## Navigation
[← Back to Overview](./overview.md)

**Related:** [Prerequisites & Setup](./prerequisites.md) | [FAQs](./faqs.md)

---

## Daily Routine Template

Use this template to structure your daily work. Adjust times based on your schedule and timezone.

```csv
Time,Activity,Duration,Notes
09:00-09:15,Daily standup,15min,Share progress blockers plans
09:15-12:00,Deep work coding,2h 45min,Focus time no meetings
12:00-13:00,Lunch break,1h,
13:00-15:00,Coding continued,2h,
15:00-15:30,Code review,30min,Review others' PRs
15:30-17:00,Learning time,1h 30min,Documentation tutorials
17:00-17:30,Documentation,30min,Update docs write learnings
17:30-18:00,Mentor sync,30min,Q&A blockers planning
```

### Tips for Daily Routine:
- **Deep work first**: Schedule your most challenging tasks during morning focus time
- **Block distractions**: Turn off notifications during deep work sessions
- **Take breaks**: Use Pomodoro technique (25min work, 5min break)
- **End-of-day reflection**: Spend 10 minutes documenting what you learned

---

## Weekly Milestones Template

Track your weekly goals and progress using this JSON structure. You can keep this in a `weekly-log.json` file or your personal notes.

```json
{
  "week": 1,
  "date_range": "2026-01-19 to 2026-01-25",
  "weekly_goals": {
    "learning": "Topics to master this week (e.g., TypeScript generics, NestJS modules)",
    "coding": "Features or projects to complete (e.g., Implement user authentication)",
    "documentation": "Docs to read or write (e.g., Read NestJS Guards documentation, document API endpoints)",
    "collaboration": "PRs to review, meetings to attend (e.g., Review 2 PRs, attend team standup)"
  },
  "weekly_review": {
    "achievements": "What went well (e.g., Successfully implemented JWT authentication, learned Prisma relations)",
    "challenges": "What was difficult (e.g., Struggled with TypeScript decorators, needed help with database migrations)",
    "learnings": "Key takeaways (e.g., Understanding the difference between authentication and authorization)",
    "next_week": "Goals for next week (e.g., Implement role-based access control, learn about testing strategies)"
  },
  "mentor_feedback": {
    "strengths": "What mentor highlighted positively",
    "areas_for_improvement": "What to focus on improving",
    "action_items": "Specific tasks or skills to work on"
  }
}
```

### How to Use Weekly Template:
1. **Monday morning**: Fill in `weekly_goals` section
2. **Throughout the week**: Take daily notes on progress
3. **Friday afternoon**: Complete `weekly_review` section
4. **After mentor sync**: Add `mentor_feedback` section
5. **Keep history**: Save each week's file (e.g., `week-01.json`, `week-02.json`)

---

## Key Tools & Commands

### Project Setup Commands

```bash
# Install all dependencies
npm install

# Generate Prisma client (after schema changes)
npx prisma generate

# Create and apply database migrations
npx prisma migrate dev

# Seed database with initial data
npx prisma db seed
```

### Development Commands

```bash
# Start development server (with hot reload)
npm run start:dev

# Run all tests
npm run test

# Run tests in watch mode (re-runs on file changes)
npm run test:watch

# Run tests with coverage report
npm run test:cov

# Run specific test file
npm run test -- user.service.spec.ts

# Run end-to-end tests
npm run test:e2e
```

### Database Commands

```bash
# Open Prisma Studio (visual database browser)
npx prisma studio

# Push schema changes without migration (dev only)
npx prisma db push

# Apply migrations in production
npx prisma migrate deploy

# Reset database (WARNING: deletes all data)
npx prisma migrate reset

# View current database status
npx prisma migrate status

# Create new empty migration
npx prisma migrate dev --create-only
```

### Code Quality Commands

```bash
# Run linter (check for code issues)
npm run lint

# Fix auto-fixable lint issues
npm run lint:fix

# Format code with Prettier
npm run format

# Check formatting without modifying files
npm run format:check

# Run type checking (TypeScript)
npm run typecheck
```

### Build & Deployment Commands

```bash
# Build for production
npm run build

# Start production server (requires build first)
npm run start:prod

# Check build output
ls -la dist/

# Clean build artifacts
rm -rf dist/
```

### Docker Commands (if using containerization)

```bash
# Build Docker image
docker build -t my-app .

# Run container
docker run -p 3000:3000 my-app

# Run with Docker Compose
docker-compose up

# Run in background
docker-compose up -d

# Stop containers
docker-compose down

# View logs
docker-compose logs -f

# Rebuild and restart
docker-compose up --build
```

---

## Git Commit Message Conventions

Follow these conventions for clear, consistent commit history:

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

| Type | Description | Example |
|------|-------------|---------|
| `feat` | New feature | `feat(auth): add JWT authentication` |
| `fix` | Bug fix | `fix(users): resolve email validation issue` |
| `docs` | Documentation only | `docs(readme): update setup instructions` |
| `style` | Code style (formatting, semicolons) | `style(api): format code with prettier` |
| `refactor` | Code refactoring (no feature/bug change) | `refactor(users): extract validation logic` |
| `test` | Add or update tests | `test(auth): add login endpoint tests` |
| `chore` | Maintenance tasks | `chore(deps): update dependencies` |
| `perf` | Performance improvement | `perf(db): add index to user queries` |
| `ci` | CI/CD changes | `ci(github): add automated testing workflow` |

### Examples

```bash
# Good commit messages
git commit -m "feat(auth): implement password reset flow"
git commit -m "fix(api): handle null values in user profile"
git commit -m "docs(api): add endpoint documentation for orders"
git commit -m "test(users): add integration tests for user service"

# Bad commit messages (avoid these)
git commit -m "fixed bug"
git commit -m "update"
git commit -m "WIP"
git commit -m "asdfasdf"
```

### Multi-line Commit Message

```bash
git commit -m "feat(orders): implement order cancellation

- Add cancel order endpoint
- Update order status enum
- Send cancellation email notification
- Add tests for cancellation flow

Closes #123"
```

---

## Pull Request Template

Use this template when creating PRs:

```markdown
## Description
Brief description of what this PR does and why.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Changes Made
- List specific changes made in this PR
- Be concise but clear
- Include technical details if relevant

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing performed
- [ ] All tests passing

## Screenshots (if applicable)
Add screenshots or GIFs for UI changes

## Related Issues
Closes #(issue number)
Related to #(issue number)

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review performed
- [ ] Code commented where necessary
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Tests added that prove fix/feature works
- [ ] Dependent changes merged

## Additional Notes
Any additional context or notes for reviewers
```

---

## Debugging Checklist

When you encounter an issue, work through this checklist systematically:

### 1. Understand the Error

```bash
# Read the full error stack trace
# Don't skip the details - they contain clues

# Common error types:
# - Syntax errors: Usually line number is exact
# - Type errors: TypeScript will point to the issue
# - Runtime errors: Check the stack trace
# - Database errors: Check Prisma logs
```

### 2. Check the Basics

- [ ] **Server running?** Check terminal for errors
- [ ] **Database connected?** Check `.env` file and database status
- [ ] **Dependencies installed?** Run `npm install` again
- [ ] **Environment variables set?** Verify `.env` file
- [ ] **Prisma client updated?** Run `npx prisma generate`
- [ ] **Migrations applied?** Run `npx prisma migrate dev`
- [ ] **Port already in use?** Try different port or kill existing process

### 3. Isolate the Problem

```typescript
// Use console.log strategically
console.log('Before database call');
console.log('User data:', userData);
console.log('After database call');

// Use debugger statements
debugger; // Execution will pause here in debug mode

// Test in isolation
// Comment out code sections to find where error occurs
```

### 4. Check Documentation

- [ ] Read official NestJS documentation
- [ ] Check Prisma documentation
- [ ] Review TypeScript handbook
- [ ] Search GitHub issues for similar problems

### 5. Ask for Help (After trying above steps)

**Good help request format:**
```markdown
**Problem**: Brief description of the issue

**Expected behavior**: What should happen

**Actual behavior**: What's happening instead

**Error message**: (paste full error with stack trace)

**What I've tried**:
1. Checked X
2. Tried Y
3. Verified Z

**Code snippet**: (relevant code that's causing the issue)

**Environment**: Node version, OS, dependencies versions
```

---

## Common Error Messages & Solutions

### TypeScript Errors

#### `Property 'X' does not exist on type 'Y'`

**Cause**: TypeScript can't find the property you're trying to access.

**Solutions:**
```typescript
// Solution 1: Check spelling and case
user.email // Correct
user.Email // Wrong - case sensitive

// Solution 2: Ensure type is correct
const user: User = await this.userRepository.findOne(...);
console.log(user.email); // TypeScript knows user is User type

// Solution 3: Check if property is optional
interface User {
  email?: string; // Optional property
}
// Access safely
const email = user.email ?? 'default@example.com';
```

#### `Cannot find module 'X'`

**Cause**: Import path is wrong or module not installed.

**Solutions:**
```bash
# Solution 1: Install missing package
npm install X

# Solution 2: Check import path
import { User } from './user.entity'; // Relative path
import { User } from '@/entities/user.entity'; // Alias path

# Solution 3: Regenerate Prisma client
npx prisma generate
```

### NestJS Errors

#### `Nest can't resolve dependencies of X`

**Cause**: Dependency injection issue - module/provider not registered.

**Solutions:**
```typescript
// Solution: Add to module's providers and imports
@Module({
  imports: [TypeOrmModule.forFeature([User])], // Add entity
  providers: [UserService, UserRepository], // Add service
  controllers: [UserController],
  exports: [UserService], // Export if used in other modules
})
export class UserModule {}
```

#### `Cannot GET /api/endpoint`

**Cause**: Route not defined or controller not registered.

**Solutions:**
```typescript
// Solution 1: Check route decorator
@Get('users') // Correct
@Get('/users') // Also works, but prefer without leading slash

// Solution 2: Check global prefix
// In main.ts
app.setGlobalPrefix('api'); // Now routes are /api/users

// Solution 3: Ensure controller is in module
@Module({
  controllers: [UserController], // Add this
})
```

### Prisma Errors

#### `Invalid prisma.X.Y() invocation`

**Cause**: Prisma query syntax error or schema mismatch.

**Solutions:**
```typescript
// Solution 1: Check field names match schema
await prisma.user.findUnique({
  where: { id: userId }, // Field name must match schema
});

// Solution 2: Regenerate Prisma client after schema changes
// Run this after every schema change
npx prisma generate

// Solution 3: Check for required fields
await prisma.user.create({
  data: {
    email: 'test@example.com',
    name: 'Test User', // Include all required fields
  },
});
```

#### `Migration failed`

**Cause**: Database schema conflict or SQL error.

**Solutions:**
```bash
# Solution 1: Reset database (DEV ONLY - deletes all data)
npx prisma migrate reset

# Solution 2: Check migration file for SQL errors
# Look in prisma/migrations/

# Solution 3: Force push schema (DEV ONLY)
npx prisma db push --force-reset
```

### Database Errors

#### `Connection refused` or `ECONNREFUSED`

**Cause**: Database not running or wrong connection string.

**Solutions:**
```bash
# Solution 1: Start PostgreSQL
# macOS
brew services start postgresql

# Linux
sudo systemctl start postgresql

# Docker
docker-compose up -d postgres

# Solution 2: Check DATABASE_URL in .env
DATABASE_URL="postgresql://user:password@localhost:5432/dbname"
#                         ↑     ↑          ↑         ↑      ↑
#                       user  password   host      port  database

# Solution 3: Test connection
psql -U username -d dbname
```

---

## VS Code Keyboard Shortcuts

### Essential Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| **Command Palette** | `Ctrl+Shift+P` | `Cmd+Shift+P` |
| **Quick File Open** | `Ctrl+P` | `Cmd+P` |
| **Go to Symbol** | `Ctrl+Shift+O` | `Cmd+Shift+O` |
| **Find in Files** | `Ctrl+Shift+F` | `Cmd+Shift+F` |
| **Go to Definition** | `F12` | `F12` |
| **Peek Definition** | `Alt+F12` | `Option+F12` |
| **Format Document** | `Shift+Alt+F` | `Shift+Option+F` |
| **Toggle Terminal** | `Ctrl+`` | `Cmd+`` |
| **Multi-cursor** | `Ctrl+Alt+Down` | `Cmd+Option+Down` |
| **Find Next** | `F3` | `Cmd+G` |
| **Rename Symbol** | `F2` | `F2` |

### Debugging Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| **Start Debugging** | `F5` | `F5` |
| **Step Over** | `F10` | `F10` |
| **Step Into** | `F11` | `F11` |
| **Step Out** | `Shift+F11` | `Shift+F11` |
| **Toggle Breakpoint** | `F9` | `F9` |

---

## Environment Variables Template

Create a `.env` file in your project root:

```bash
# Database
DATABASE_URL="postgresql://username:password@localhost:5432/database_name"

# Application
NODE_ENV="development"
PORT=3000
API_PREFIX="api"

# JWT Authentication
JWT_SECRET="your-super-secret-jwt-key-change-this-in-production"
JWT_EXPIRES_IN="7d"
JWT_REFRESH_SECRET="your-refresh-token-secret"
JWT_REFRESH_EXPIRES_IN="30d"

# Email (if using email features)
SMTP_HOST="smtp.gmail.com"
SMTP_PORT=587
SMTP_USER="your-email@gmail.com"
SMTP_PASSWORD="your-app-password"
EMAIL_FROM="noreply@yourapp.com"

# Redis (if using caching)
REDIS_HOST="localhost"
REDIS_PORT=6379
REDIS_PASSWORD=""

# AWS S3 (if using file uploads)
AWS_ACCESS_KEY_ID="your-access-key"
AWS_SECRET_ACCESS_KEY="your-secret-key"
AWS_REGION="us-east-1"
AWS_S3_BUCKET="your-bucket-name"

# Third-party APIs
STRIPE_API_KEY="sk_test_..."
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# Logging
LOG_LEVEL="debug"
```

### Security Notes:
- **Never commit `.env` to Git** - add to `.gitignore`
- **Use different values for production** - never use dev secrets in prod
- **Rotate secrets regularly** - especially JWT secrets
- **Use environment variables in CI/CD** - don't hardcode secrets

---

## Docker Compose Template

If your project uses Docker, here's a basic `docker-compose.yml`:

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    container_name: internship_db
    restart: always
    environment:
      POSTGRES_USER: devuser
      POSTGRES_PASSWORD: devpassword
      POSTGRES_DB: internship_db
    ports:
      - '5432:5432'
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    container_name: internship_redis
    restart: always
    ports:
      - '6379:6379'
    volumes:
      - redis_data:/data

  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: internship_app
    restart: always
    environment:
      DATABASE_URL: postgresql://devuser:devpassword@postgres:5432/internship_db
      REDIS_HOST: redis
      NODE_ENV: development
    ports:
      - '3000:3000'
    depends_on:
      - postgres
      - redis
    volumes:
      - .:/app
      - /app/node_modules

volumes:
  postgres_data:
  redis_data:
```

---

## Testing Patterns Cheat Sheet

### Unit Test Structure

```typescript
describe('UserService', () => {
  let service: UserService;
  let repository: MockType<Repository<User>>;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UserService,
        {
          provide: getRepositoryToken(User),
          useFactory: repositoryMockFactory,
        },
      ],
    }).compile();

    service = module.get<UserService>(UserService);
    repository = module.get(getRepositoryToken(User));
  });

  describe('findOne', () => {
    it('should return a user when found', async () => {
      const mockUser = { id: 1, email: 'test@example.com' };
      repository.findOne.mockReturnValue(mockUser);

      const result = await service.findOne(1);

      expect(result).toEqual(mockUser);
      expect(repository.findOne).toHaveBeenCalledWith({ where: { id: 1 } });
    });

    it('should throw NotFoundException when user not found', async () => {
      repository.findOne.mockReturnValue(null);

      await expect(service.findOne(999)).rejects.toThrow(NotFoundException);
    });
  });
});
```

### E2E Test Structure

```typescript
describe('AuthController (e2e)', () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  afterAll(async () => {
    await app.close();
  });

  it('/auth/login (POST)', () => {
    return request(app.getHttpServer())
      .post('/auth/login')
      .send({ email: 'test@example.com', password: 'password123' })
      .expect(200)
      .expect((res) => {
        expect(res.body).toHaveProperty('access_token');
        expect(res.body).toHaveProperty('user');
      });
  });
});
```

---

## Prisma Schema Examples

### Common Patterns

```prisma
// User with relations
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  password  String
  role      Role     @default(USER)
  posts     Post[]
  profile   Profile?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([email])
  @@map("users")
}

// One-to-One relation
model Profile {
  id     Int    @id @default(autoincrement())
  bio    String
  avatar String?
  userId Int    @unique
  user   User   @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@map("profiles")
}

// One-to-Many relation
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String
  published Boolean  @default(false)
  authorId  Int
  author    User     @relation(fields: [authorId], references: [id])
  tags      Tag[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([authorId])
  @@map("posts")
}

// Many-to-Many relation
model Tag {
  id    Int    @id @default(autoincrement())
  name  String @unique
  posts Post[]

  @@map("tags")
}

// Enum
enum Role {
  USER
  ADMIN
  MODERATOR
}
```

---

## Navigation
[← Back to Overview](./overview.md)

**Related:** [Prerequisites & Setup](./prerequisites.md) | [FAQs](./faqs.md) | [Evaluation Criteria](./evaluation.md)

---

**Tip**: Bookmark this page! You'll refer to it frequently throughout your internship. Consider keeping a local copy of the templates and customizing them for your workflow.
