---
sidebar_position: 6
---

# Architecture & System Design

Learn to design scalable, maintainable, and robust backend systems.

## Architecture Patterns

### 1. Monolithic Architecture

Single, unified application where all components are tightly coupled.

**Structure:**
```
├── app/
│   ├── controllers/
│   ├── models/
│   ├── services/
│   ├── routes/
│   └── middleware/
├── config/
├── database/
└── server.js
```

**Pros:**
- Simple to develop and deploy
- Easy to test locally
- No network latency between components
- Straightforward debugging

**Cons:**
- Difficult to scale specific parts
- Tight coupling makes changes risky
- Long deployment times
- Single point of failure

**When to use:**
- MVP or early-stage projects
- Small teams
- Simple applications
- When you need fast development

---

### 2. Microservices Architecture

Application split into small, independent services that communicate via APIs.

**Structure:**
```
├── user-service/
│   ├── src/
│   └── package.json
├── product-service/
│   ├── src/
│   └── package.json
├── order-service/
│   ├── src/
│   └── package.json
└── api-gateway/
    ├── src/
    └── package.json
```

**Pros:**
- Scale services independently
- Technology flexibility per service
- Easier to maintain and update
- Fault isolation (one service fails, others continue)
- Team autonomy

**Cons:**
- Complex deployment and monitoring
- Network latency
- Data consistency challenges
- Requires DevOps expertise
- Testing complexity

**When to use:**
- Large, complex applications
- Need to scale specific features
- Multiple teams working independently
- Long-term, evolving products

---

### 3. Serverless Architecture

Run code without managing servers. Functions triggered by events.

**Example Services:**
- AWS Lambda
- Google Cloud Functions
- Azure Functions
- Cloudflare Workers
- Vercel Functions

**Pros:**
- No server management
- Pay per execution (cost-effective for low traffic)
- Auto-scaling
- Focus on code, not infrastructure

**Cons:**
- Cold start latency
- Vendor lock-in
- Limited execution time
- Debugging challenges
- Can be expensive at scale

**When to use:**
- Event-driven workloads
- Variable traffic patterns
- Microservices/API endpoints
- Background jobs

---

## Design Patterns

### 1. MVC (Model-View-Controller)

Separates application into three interconnected components.

```javascript
// Model (data layer)
class User {
  static async findById(id) {
    return await db.users.findById(id);
  }
  
  static async create(data) {
    return await db.users.create(data);
  }
}

// Controller (business logic)
class UserController {
  async getUser(req, res) {
    const user = await User.findById(req.params.id);
    res.json(user);
  }
  
  async createUser(req, res) {
    const user = await User.create(req.body);
    res.status(201).json(user);
  }
}

// Routes (entry points)
router.get('/users/:id', userController.getUser);
router.post('/users', userController.createUser);
```

---

### 2. Repository Pattern

Abstracts data access logic from business logic.

```javascript
// Repository (data access layer)
class UserRepository {
  async findById(id) {
    return await db.users.findOne({ id });
  }
  
  async findByEmail(email) {
    return await db.users.findOne({ email });
  }
  
  async create(userData) {
    return await db.users.insert(userData);
  }
  
  async update(id, userData) {
    return await db.users.update({ id }, userData);
  }
}

// Service (business logic)
class UserService {
  constructor(userRepository) {
    this.userRepository = userRepository;
  }
  
  async registerUser(email, password) {
    // Check if user exists
    const existing = await this.userRepository.findByEmail(email);
    if (existing) {
      throw new Error('User already exists');
    }
    
    // Hash password
    const passwordHash = await bcrypt.hash(password, 10);
    
    // Create user
    return await this.userRepository.create({
      email,
      passwordHash,
      createdAt: new Date()
    });
  }
}

// Controller
class UserController {
  constructor(userService) {
    this.userService = userService;
  }
  
  async register(req, res) {
    try {
      const user = await this.userService.registerUser(
        req.body.email,
        req.body.password
      );
      res.status(201).json(user);
    } catch (error) {
      res.status(400).json({ error: error.message });
    }
  }
}
```

---

### 3. Service Layer Pattern

Encapsulates business logic in service classes.

```javascript
// Services for different domains
class OrderService {
  async createOrder(userId, items) {
    // Validate items
    const products = await ProductService.validateProducts(items);
    
    // Check inventory
    await InventoryService.checkAvailability(items);
    
    // Calculate total
    const total = this.calculateTotal(products, items);
    
    // Create order
    const order = await OrderRepository.create({
      userId,
      items,
      total,
      status: 'pending'
    });
    
    // Send notification
    await NotificationService.sendOrderConfirmation(userId, order);
    
    return order;
  }
  
  calculateTotal(products, items) {
    return items.reduce((sum, item) => {
      const product = products.find(p => p.id === item.productId);
      return sum + (product.price * item.quantity);
    }, 0);
  }
}
```

---

## System Design Concepts

### 1. Load Balancing

Distribute traffic across multiple servers.

**Algorithms:**
- **Round Robin**: Distribute requests evenly
- **Least Connections**: Send to server with fewest active connections
- **IP Hash**: Route based on client IP
- **Weighted**: Distribute based on server capacity

**Tools:**
- NGINX
- HAProxy
- AWS Elastic Load Balancer
- Cloudflare Load Balancing

---

### 2. Caching Strategies

Improve performance by storing frequently accessed data.

**Levels of Caching:**
```
Client → CDN → API Gateway → Application Cache → Database Cache
```

**Caching Patterns:**

**Cache-Aside (Lazy Loading):**
```javascript
async function getUser(id) {
  // Check cache first
  let user = await cache.get(`user:${id}`);
  
  if (!user) {
    // Cache miss - fetch from database
    user = await db.users.findById(id);
    
    // Store in cache
    await cache.set(`user:${id}`, user, { ttl: 3600 });
  }
  
  return user;
}
```

**Write-Through:**
```javascript
async function updateUser(id, data) {
  // Update database
  const user = await db.users.update(id, data);
  
  // Update cache
  await cache.set(`user:${id}`, user, { ttl: 3600 });
  
  return user;
}
```

**Cache Invalidation:**
```javascript
async function deleteUser(id) {
  await db.users.delete(id);
  await cache.del(`user:${id}`); // Invalidate cache
}
```

---

### 3. Message Queues

Asynchronous communication between services.

**Use Cases:**
- Email sending
- Image processing
- Report generation
- Data synchronization

**Popular Tools:**
- RabbitMQ
- Redis (pub/sub)
- AWS SQS
- Apache Kafka
- Bull (Node.js)

**Example (Bull Queue):**
```javascript
const Queue = require('bull');
const emailQueue = new Queue('email', process.env.REDIS_URL);

// Producer (add job to queue)
app.post('/register', async (req, res) => {
  const user = await createUser(req.body);
  
  // Add email job to queue
  await emailQueue.add('welcome', {
    email: user.email,
    name: user.name
  });
  
  res.json(user);
});

// Worker (process jobs)
emailQueue.process('welcome', async (job) => {
  await sendWelcomeEmail(job.data.email, job.data.name);
});
```

---

### 4. Database Replication

Copy data across multiple database servers.

**Master-Slave Replication:**
- **Master**: Handles writes
- **Slaves**: Handle reads
- Improves read performance

```javascript
// Write to master
await masterDb.users.create(userData);

// Read from slave
const users = await slaveDb.users.findAll();
```

**Multi-Master Replication:**
- Multiple writable databases
- Conflict resolution needed

---

### 5. Horizontal vs Vertical Scaling

**Vertical Scaling (Scale Up):**
- Add more power to existing server (CPU, RAM)
- Simpler but has limits
- Single point of failure

**Horizontal Scaling (Scale Out):**
- Add more servers
- Requires load balancer
- More complex but unlimited scale
- Better fault tolerance

---

### 6. API Gateway Pattern

Single entry point for all client requests.

**Responsibilities:**
- Routing
- Authentication
- Rate limiting
- Request/response transformation
- Caching
- Monitoring

**Example (Express Gateway):**
```yaml
apiEndpoints:
  users:
    host: api.example.com
    paths: '/users/*'
  orders:
    host: api.example.com
    paths: '/orders/*'

serviceEndpoints:
  userService:
    url: 'http://user-service:3000'
  orderService:
    url: 'http://order-service:3001'

policies:
  - basic-auth
  - rate-limit
  - proxy

pipelines:
  usersPipeline:
    apiEndpoints:
      - users
    policies:
      - rate-limit:
          max: 100
          windowMs: 60000
      - proxy:
          action:
            serviceEndpoint: userService
```

---

### 7. Circuit Breaker Pattern

Prevent cascading failures when a service is down.

```javascript
class CircuitBreaker {
  constructor(service, threshold = 5, timeout = 60000) {
    this.service = service;
    this.threshold = threshold;
    this.timeout = timeout;
    this.failureCount = 0;
    this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    this.nextAttempt = Date.now();
  }
  
  async call(...args) {
    if (this.state === 'OPEN') {
      if (Date.now() < this.nextAttempt) {
        throw new Error('Circuit breaker is OPEN');
      }
      this.state = 'HALF_OPEN';
    }
    
    try {
      const result = await this.service(...args);
      this.onSuccess();
      return result;
    } catch (error) {
      this.onFailure();
      throw error;
    }
  }
  
  onSuccess() {
    this.failureCount = 0;
    this.state = 'CLOSED';
  }
  
  onFailure() {
    this.failureCount++;
    if (this.failureCount >= this.threshold) {
      this.state = 'OPEN';
      this.nextAttempt = Date.now() + this.timeout;
    }
  }
}

// Usage
const breaker = new CircuitBreaker(externalApiCall);
try {
  const data = await breaker.call(params);
} catch (error) {
  // Fallback logic
  return cachedData;
}
```

---

## Database Strategies

### 1. Database Sharding

Split data across multiple databases.

**Strategies:**
- **Horizontal Sharding**: Split rows (users 1-1000 → DB1, 1001-2000 → DB2)
- **Vertical Sharding**: Split columns (user profile → DB1, user activity → DB2)
- **Geographic Sharding**: Split by region (US users → US DB, EU users → EU DB)

### 2. Read Replicas

Multiple read-only copies of the database.

```javascript
const writableDb = createConnection(MASTER_DB_URL);
const readOnlyDb = createConnection(REPLICA_DB_URL);

// Writes go to master
await writableDb.users.create(userData);

// Reads from replica
const users = await readOnlyDb.users.findAll();
```

---

## Monitoring & Observability

### 1. Logging

Structure your logs for better debugging.

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

// Log with context
logger.info('User registered', {
  userId: user.id,
  email: user.email,
  timestamp: new Date()
});
```

### 2. Metrics

Track application performance.

**Key Metrics:**
- Request rate
- Error rate
- Response time (p50, p95, p99)
- Database query time
- CPU/Memory usage

**Tools:**
- Prometheus
- Grafana
- Datadog
- New Relic

### 3. Distributed Tracing

Track requests across multiple services.

**Tools:**
- Jaeger
- Zipkin
- AWS X-Ray

---

## Learning Path

1. **Start with monolith**
   - Build a complete application
   - Understand all components working together

2. **Learn design patterns**
   - Implement MVC, Repository, Service patterns
   - Separate concerns properly

3. **Add caching**
   - Implement Redis caching
   - Understand cache invalidation

4. **Explore async processing**
   - Use message queues for background jobs
   - Understand event-driven architecture

5. **Study microservices**
   - Break monolith into services
   - Learn service communication patterns

6. **Master deployment**
   - Containerize with Docker
   - Orchestrate with Kubernetes
   - Set up monitoring and logging

## Practice Projects

1. **Monolithic Blog** - Full-featured with MVC pattern
2. **E-commerce API** - Repository and service patterns
3. **Task Queue System** - Async processing with Redis/Bull
4. **Microservices App** - Split into user, product, order services
5. **API Gateway** - Route requests to multiple backends

## Next Steps

- **Deployment**: Learn Docker, Kubernetes, CI/CD
- **Advanced Topics**: Event sourcing, CQRS, DDD
