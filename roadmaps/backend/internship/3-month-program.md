---
id: 3-month-program
---

# 3-Month Internship Program

**Objective**: Master advanced backend engineering and build a production-grade application with full ownership.

## Navigation
[← Back to Overview](./overview.md) | [← 2-Month Program](./2-month-program.md)

**Related:** [Tech Stack](./tech-stack.md) | [Skills Matrix](./skills-matrix.md) | Architecture Patterns

---

## Program Structure

This 3-month program follows a **two-phase approach**:

```
┌──────────────────────────────────────────────────────────┐
│              3-MONTH INTERNSHIP PROGRAM                  │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  PHASE 1: LEARNING (Weeks 1-5)                          │
│  ├─ Weeks 1-3: Advanced patterns + architecture         │
│  ├─ Weeks 4-5: System design + user research            │
│  └─ Build: Real-time task API + design document         │
│                                                          │
│  PHASE 2: FINAL PROJECT (Weeks 6-12)                    │
│  Choose ONE option:                                      │
│  ├─ 🏢 Internal System (Enterprise-grade tool)          │
│  ├─ 🌍 Real-World App (Complex module ownership)        │
│  └─ 🚀 New Application (Full platform from scratch)     │
│                                                          │
│  ├─ Weeks 6-7: User research + technical design         │
│  ├─ Weeks 8-10: Core development sprint                 │
│  └─ Weeks 11-12: Production deployment + handover       │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### What You'll Achieve

By the end of this program:
- ✅ Master advanced architecture patterns (Clean Architecture, CQRS, Event-Driven)
- ✅ Build complex systems with Redis, WebSockets, background jobs
- ✅ Design scalable applications from user research to deployment
- ✅ Own complete SDLC (discovery → design → dev → test → deploy → maintain)
- ✅ Deploy production systems with monitoring and observability
- ✅ Be **ready for mid-to-senior level roles**

---

## PHASE 1: Learning Phase (Weeks 1-5)

**Objective:** Master advanced backend engineering concepts

### Weeks 1-3: Advanced Patterns & Architecture

### 🎯 Learning Objectives / Competencies

By the end of Phase 1, you will be able to:
- **All competencies from 1-2 month programs**
- Design applications using clean architecture principles
- Implement CQRS (Command Query Responsibility Segregation) pattern
- Use Redis for caching and session management
- Implement background job processing with Bull queues
- Build real-time features with WebSockets
- Practice Test-Driven Development (TDD)
- Conduct security hardening and performance optimization

### 📚 Core Materials

```json
{
  "weeks": [
    {
      "week": 1,
      "topics": [
        "Advanced TypeScript patterns",
        "NestJS architecture deep dive",
        "Design patterns (Repository, Factory, Strategy)",
        "SOLID principles",
        "Clean architecture"
      ],
      "hours": 40
    },
    {
      "week": 2,
      "topics": [
        "Advanced Prisma (transactions, raw queries, performance)",
        "Redis for caching",
        "Bull queues for background jobs",
        "WebSockets for real-time features",
        "Event-driven architecture"
      ],
      "hours": 40
    },
    {
      "week": 3,
      "topics": [
        "Testing strategies (TDD, BDD)",
        "E2E testing with Supertest",
        "Mocking and test fixtures",
        "Security hardening",
        "Performance optimization"
      ],
      "hours": 40
    }
  ]
}
```

### 🔨 Learning Activities

#### Week 1: Architecture Mastery (40 hours)

**Day 1-2: Clean Architecture & SOLID (16 hours)**

**Goals:**
- Understand clean architecture layers
- Apply SOLID principles
- Implement scalable project structure

**Topics:**

1. **Clean Architecture Layers (6 hours)**
   ```
   ┌─────────────────────────────────────┐
   │        Presentation Layer           │  ← Controllers, DTOs
   │     (NestJS Controllers)            │
   └──────────────┬──────────────────────┘
                  │
   ┌──────────────▼──────────────────────┐
   │       Application Layer             │  ← Use Cases, Services
   │     (Business Logic)                │
   └──────────────┬──────────────────────┘
                  │
   ┌──────────────▼──────────────────────┐
   │          Domain Layer               │  ← Entities, Interfaces
   │     (Core Business Rules)           │
   └──────────────┬──────────────────────┘
                  │
   ┌──────────────▼──────────────────────┐
   │     Infrastructure Layer            │  ← Database, External APIs
   │     (Prisma, Redis, etc.)           │
   └─────────────────────────────────────┘
   ```

   **Implementation:**
   ```typescript
   // Domain Layer - Entity
   export class Task {
     constructor(
       public readonly id: string,
       public title: string,
       public description: string,
       public status: TaskStatus,
       public assigneeId?: string,
     ) {}

     complete() {
       if (this.status === TaskStatus.COMPLETED) {
         throw new Error('Task already completed');
       }
       this.status = TaskStatus.COMPLETED;
     }

     assign(userId: string) {
       this.assigneeId = userId;
     }
   }

   // Domain Layer - Repository Interface
   export interface ITaskRepository {
     findById(id: string): Promise<Task | null>;
     findAll(filters: TaskFilters): Promise<Task[]>;
     save(task: Task): Promise<void>;
     delete(id: string): Promise<void>;
   }

   // Application Layer - Use Case
   export class CreateTaskUseCase {
     constructor(private taskRepository: ITaskRepository) {}

     async execute(command: CreateTaskCommand): Promise<Task> {
       const task = new Task(
         uuid(),
         command.title,
         command.description,
         TaskStatus.TODO,
       );

       await this.taskRepository.save(task);
       return task;
     }
   }

   // Infrastructure Layer - Repository Implementation
   @Injectable()
   export class PrismaTaskRepository implements ITaskRepository {
     constructor(private prisma: PrismaService) {}

     async findById(id: string): Promise<Task | null> {
       const data = await this.prisma.task.findUnique({ where: { id } });
       return data ? this.toDomain(data) : null;
     }

     async save(task: Task): Promise<void> {
       await this.prisma.task.upsert({
         where: { id: task.id },
         create: this.toPrisma(task),
         update: this.toPrisma(task),
       });
     }

     private toDomain(data: PrismaTask): Task {
       return new Task(data.id, data.title, data.description, data.status, data.assigneeId);
     }

     private toPrisma(task: Task): Prisma.TaskCreateInput {
       return {
         id: task.id,
         title: task.title,
         description: task.description,
         status: task.status,
         assigneeId: task.assigneeId,
       };
     }
   }

   // Presentation Layer - Controller
   @Controller('tasks')
   export class TasksController {
     constructor(private createTaskUseCase: CreateTaskUseCase) {}

     @Post()
     async create(@Body() dto: CreateTaskDto) {
       const command = new CreateTaskCommand(dto.title, dto.description);
       const task = await this.createTaskUseCase.execute(command);
       return TaskResponseDto.fromDomain(task);
     }
   }
   ```

2. **SOLID Principles in Practice (6 hours)**
   ```typescript
   // S - Single Responsibility
   // ❌ Bad: Service does too much
   class UserService {
     createUser() {}
     sendEmail() {}
     generatePDF() {}
     uploadToS3() {}
   }

   // ✅ Good: Each class has one responsibility
   class UserService {
     createUser() {}
   }
   class EmailService {
     send() {}
   }
   class PDFService {
     generate() {}
   }
   class StorageService {
     upload() {}
   }

   // O - Open/Closed Principle
   // ✅ Open for extension, closed for modification
   interface NotificationSender {
     send(message: string): Promise<void>;
   }

   class EmailNotificationSender implements NotificationSender {
     async send(message: string) {
       // Send email
     }
   }

   class SMSNotificationSender implements NotificationSender {
     async send(message: string) {
       // Send SMS
     }
   }

   class NotificationService {
     constructor(private senders: NotificationSender[]) {}

     async notifyAll(message: string) {
       await Promise.all(this.senders.map(s => s.send(message)));
     }
   }

   // L - Liskov Substitution
   // ✅ Derived classes should be substitutable for base classes
   abstract class PaymentProcessor {
     abstract process(amount: number): Promise<void>;
   }

   class StripePaymentProcessor extends PaymentProcessor {
     async process(amount: number) {
       // Stripe implementation
     }
   }

   class PayPalPaymentProcessor extends PaymentProcessor {
     async process(amount: number) {
       // PayPal implementation
     }
   }

   // I - Interface Segregation
   // ✅ Many specific interfaces > one general interface
   interface Readable {
     read(): Promise<Data>;
   }

   interface Writable {
     write(data: Data): Promise<void>;
   }

   interface Deletable {
     delete(id: string): Promise<void>;
   }

   // Implement only what you need
   class ReadOnlyRepository implements Readable {
     async read() { /* ... */ }
   }

   class FullRepository implements Readable, Writable, Deletable {
     async read() { /* ... */ }
     async write() { /* ... */ }
     async delete() { /* ... */ }
   }

   // D - Dependency Inversion
   // ✅ Depend on abstractions, not concretions
   interface ILogger {
     log(message: string): void;
   }

   class ConsoleLogger implements ILogger {
     log(message: string) {
       console.log(message);
     }
   }

   class FileLogger implements ILogger {
     log(message: string) {
       // Write to file
     }
   }

   class UserService {
     constructor(private logger: ILogger) {} // Depends on abstraction

     createUser() {
       this.logger.log('User created');
     }
   }
   ```

3. **Design Patterns (4 hours)**
   ```typescript
   // Factory Pattern
   interface Task {
     execute(): void;
   }

   class EmailTask implements Task {
     execute() {
       console.log('Sending email...');
     }
   }

   class PDFTask implements Task {
     execute() {
       console.log('Generating PDF...');
     }
   }

   class TaskFactory {
     static create(type: string): Task {
       switch (type) {
         case 'email':
           return new EmailTask();
         case 'pdf':
           return new PDFTask();
         default:
           throw new Error(`Unknown task type: ${type}`);
       }
     }
   }

   // Strategy Pattern
   interface PricingStrategy {
     calculate(basePrice: number): number;
   }

   class RegularPricing implements PricingStrategy {
     calculate(basePrice: number) {
       return basePrice;
     }
   }

   class DiscountPricing implements PricingStrategy {
     constructor(private discount: number) {}

     calculate(basePrice: number) {
       return basePrice * (1 - this.discount);
     }
   }

   class PricingContext {
     constructor(private strategy: PricingStrategy) {}

     setStrategy(strategy: PricingStrategy) {
       this.strategy = strategy;
     }

     calculatePrice(basePrice: number) {
       return this.strategy.calculate(basePrice);
     }
   }

   // Repository Pattern (already shown above)
   // Observer Pattern (for events)
   interface Observer {
     update(data: any): void;
   }

   class Subject {
     private observers: Observer[] = [];

     attach(observer: Observer) {
       this.observers.push(observer);
     }

     detach(observer: Observer) {
       const index = this.observers.indexOf(observer);
       if (index > -1) this.observers.splice(index, 1);
     }

     notify(data: any) {
       this.observers.forEach(o => o.update(data));
     }
   }
   ```

**Day 3-5: CQRS Pattern & Event Sourcing Intro (24 hours)**

**Goals:**
- Understand CQRS pattern
- Separate read and write models
- Implement event-driven architecture

**Project: CQRS Task Management System**

```typescript
// Commands - Write operations
export class CreateTaskCommand {
  constructor(
    public readonly title: string,
    public readonly description: string,
  ) {}
}

export class CompleteTaskCommand {
  constructor(public readonly taskId: string) {}
}

// Command Handlers
@Injectable()
export class CreateTaskHandler {
  constructor(
    private taskRepository: ITaskRepository,
    private eventBus: EventBus,
  ) {}

  async execute(command: CreateTaskCommand): Promise<string> {
    const task = Task.create(command.title, command.description);
    await this.taskRepository.save(task);

    // Emit event
    this.eventBus.publish(new TaskCreatedEvent(task.id, task.title));

    return task.id;
  }
}

// Queries - Read operations
export class GetTaskQuery {
  constructor(public readonly taskId: string) {}
}

export class GetTasksQuery {
  constructor(public readonly filters: TaskFilters) {}
}

// Query Handlers
@Injectable()
export class GetTaskHandler {
  constructor(private prisma: PrismaService) {}

  async execute(query: GetTaskQuery): Promise<TaskReadModel> {
    // Read from optimized read model
    return this.prisma.taskReadModel.findUnique({
      where: { id: query.taskId },
    });
  }
}

// Events
export class TaskCreatedEvent {
  constructor(
    public readonly taskId: string,
    public readonly title: string,
  ) {}
}

// Event Handlers
@Injectable()
export class TaskCreatedHandler {
  constructor(
    private notificationService: NotificationService,
    private analyticsService: AnalyticsService,
  ) {}

  @OnEvent('task.created')
  async handle(event: TaskCreatedEvent) {
    // Side effects
    await this.notificationService.notify(`Task "${event.title}" created`);
    await this.analyticsService.track('task_created', { id: event.taskId });
  }
}

// Controller - Separates commands and queries
@Controller('tasks')
export class TasksController {
  constructor(
    private commandBus: CommandBus,
    private queryBus: QueryBus,
  ) {}

  @Post()
  async create(@Body() dto: CreateTaskDto) {
    const command = new CreateTaskCommand(dto.title, dto.description);
    const taskId = await this.commandBus.execute(command);
    return { id: taskId };
  }

  @Get(':id')
  async getOne(@Param('id') id: string) {
    const query = new GetTaskQuery(id);
    return this.queryBus.execute(query);
  }

  @Get()
  async getAll(@Query() filters: TaskFilters) {
    const query = new GetTasksQuery(filters);
    return this.queryBus.execute(query);
  }
}
```

**Deliverable:** Refactored task management system using clean architecture and CQRS

---

#### Week 2: Advanced Integrations (40 hours)

**Day 1-2: Redis Caching (16 hours)**

**Goals:**
- Set up Redis
- Implement caching strategies
- Use Redis for sessions

**Topics:**

1. **Redis Setup (2 hours)**
   ```bash
   npm install @nestjs/cache-manager cache-manager-redis-store redis
   ```

   ```typescript
   // redis.module.ts
   @Module({
     imports: [
       CacheModule.register({
         store: redisStore,
         host: process.env.REDIS_HOST,
         port: process.env.REDIS_PORT,
       }),
     ],
   })
   export class RedisModule {}
   ```

2. **Caching Strategies (8 hours)**
   ```typescript
   // Cache-Aside Pattern
   @Injectable()
   export class TasksService {
     constructor(
       @Inject(CACHE_MANAGER) private cacheManager: Cache,
       private prisma: PrismaService,
     ) {}

     async getTask(id: string): Promise<Task> {
       // Try cache first
       const cached = await this.cacheManager.get<Task>(`task:${id}`);
       if (cached) return cached;

       // Cache miss - fetch from DB
       const task = await this.prisma.task.findUnique({ where: { id } });
       
       // Store in cache
       await this.cacheManager.set(`task:${id}`, task, 3600); // 1 hour TTL
       
       return task;
     }

     async updateTask(id: string, data: UpdateTaskDto): Promise<Task> {
       const task = await this.prisma.task.update({
         where: { id },
         data,
       });

       // Invalidate cache
       await this.cacheManager.del(`task:${id}`);
       
       return task;
     }
   }

   // Write-Through Pattern
   async createTask(data: CreateTaskDto): Promise<Task> {
     const task = await this.prisma.task.create({ data });
     
     // Write to cache immediately
     await this.cacheManager.set(`task:${task.id}`, task, 3600);
     
     return task;
   }

   // Cache decorator
   function Cacheable(ttl: number = 3600) {
     return function (
       target: any,
       propertyKey: string,
       descriptor: PropertyDescriptor,
     ) {
       const originalMethod = descriptor.value;

       descriptor.value = async function (...args: any[]) {
         const cacheKey = `${propertyKey}:${JSON.stringify(args)}`;
         const cacheManager = this.cacheManager;

         const cached = await cacheManager.get(cacheKey);
         if (cached) return cached;

         const result = await originalMethod.apply(this, args);
         await cacheManager.set(cacheKey, result, ttl);

         return result;
       };

       return descriptor;
     };
   }

   // Usage
   @Cacheable(1800) // 30 minutes
   async getTaskStats(userId: string) {
     // Expensive operation
   }
   ```

3. **Session Management (6 hours)**
   ```typescript
   // Session store with Redis
   import * as session from 'express-session';
   import * as connectRedis from 'connect-redis';
   import { createClient } from 'redis';

   const RedisStore = connectRedis(session);
   const redisClient = createClient({
     host: process.env.REDIS_HOST,
     port: Number(process.env.REDIS_PORT),
   });

   app.use(
     session({
       store: new RedisStore({ client: redisClient }),
       secret: process.env.SESSION_SECRET,
       resave: false,
       saveUninitialized: false,
       cookie: {
         secure: process.env.NODE_ENV === 'production',
         httpOnly: true,
         maxAge: 1000 * 60 * 60 * 24 * 7, // 7 days
       },
     }),
   );
   ```

**Day 3-4: Background Jobs with Bull (16 hours)**

**Goals:**
- Set up Bull queues
- Implement job processors
- Handle job failures and retries

**Topics:**

1. **Bull Setup (3 hours)**
   ```bash
   npm install @nestjs/bull bull
   ```

   ```typescript
   // app.module.ts
   @Module({
     imports: [
       BullModule.forRoot({
         redis: {
           host: process.env.REDIS_HOST,
           port: Number(process.env.REDIS_PORT),
         },
       }),
       BullModule.registerQueue(
         { name: 'email' },
         { name: 'pdf' },
         { name: 'analytics' },
       ),
     ],
   })
   ```

2. **Job Producers (5 hours)**
   ```typescript
   // email.service.ts
   @Injectable()
   export class EmailService {
     constructor(
       @InjectQueue('email') private emailQueue: Queue,
     ) {}

     async sendWelcomeEmail(user: User) {
       await this.emailQueue.add('welcome', {
         email: user.email,
         name: user.name,
       }, {
         attempts: 3,
         backoff: {
           type: 'exponential',
           delay: 2000,
         },
       });
     }

     async sendInvoice(invoiceId: string) {
       await this.emailQueue.add('invoice', {
         invoiceId,
       }, {
         priority: 1, // High priority
         delay: 5000, // Send after 5 seconds
       });
     }
   }
   ```

3. **Job Processors (8 hours)**
   ```typescript
   // email.processor.ts
   @Processor('email')
   export class EmailProcessor {
     private readonly logger = new Logger(EmailProcessor.name);

     constructor(
       private emailProvider: EmailProvider,
       private prisma: PrismaService,
     ) {}

     @Process('welcome')
     async sendWelcomeEmail(job: Job<{ email: string; name: string }>) {
       this.logger.log(`Sending welcome email to ${job.data.email}`);

       try {
         await this.emailProvider.send({
           to: job.data.email,
           subject: 'Welcome!',
           template: 'welcome',
           context: { name: job.data.name },
         });

         this.logger.log(`Welcome email sent successfully`);
       } catch (error) {
         this.logger.error(`Failed to send email: ${error.message}`);
         throw error; // Will retry based on job config
       }
     }

     @Process('invoice')
     async sendInvoice(job: Job<{ invoiceId: string }>) {
       const { invoiceId } = job.data;

       // Generate PDF
       const pdf = await this.generatePDF(invoiceId);

       // Send email with attachment
       await this.emailProvider.send({
         to: invoice.customer.email,
         subject: `Invoice ${invoice.number}`,
         template: 'invoice',
         attachments: [{ filename: 'invoice.pdf', content: pdf }],
       });
     }

     @OnQueueCompleted()
     onCompleted(job: Job) {
       this.logger.log(`Job ${job.id} completed`);
     }

     @OnQueueFailed()
     onFailed(job: Job, error: Error) {
       this.logger.error(`Job ${job.id} failed: ${error.message}`);
       // Could send alert, log to monitoring service, etc.
     }
   }
   ```

**Day 5: WebSockets & Real-time Features (8 hours)**

**Goals:**
- Set up WebSocket gateway
- Implement real-time notifications
- Build live updates

**Topics:**

1. **WebSocket Setup (2 hours)**
   ```bash
   npm install @nestjs/websockets @nestjs/platform-socket.io socket.io
   ```

2. **Gateway Implementation (6 hours)**
   ```typescript
   // tasks.gateway.ts
   @WebSocketGateway({
     cors: {
       origin: process.env.CLIENT_URL,
       credentials: true,
     },
   })
   export class TasksGateway implements OnGatewayConnection, OnGatewayDisconnect {
     @WebSocketServer()
     server: Server;

     private readonly logger = new Logger(TasksGateway.name);

     constructor(
       private jwtService: JwtService,
     ) {}

     async handleConnection(client: Socket) {
       try {
         // Authenticate
         const token = client.handshake.auth.token;
         const payload = await this.jwtService.verifyAsync(token);
         
         client.data.userId = payload.sub;
         
         // Join user-specific room
         client.join(`user:${payload.sub}`);
         
         this.logger.log(`Client connected: ${client.id}`);
       } catch {
         client.disconnect();
       }
     }

     handleDisconnect(client: Socket) {
       this.logger.log(`Client disconnected: ${client.id}`);
     }

     // Server -> Client events
     notifyTaskCreated(userId: string, task: Task) {
       this.server.to(`user:${userId}`).emit('task:created', task);
     }

     notifyTaskUpdated(taskId: string, updates: Partial<Task>) {
       this.server.emit('task:updated', { taskId, updates });
     }

     broadcastMessage(message: string) {
       this.server.emit('notification', { message, timestamp: new Date() });
     }

     // Client -> Server events
     @SubscribeMessage('task:subscribe')
     handleSubscribe(client: Socket, taskId: string) {
       client.join(`task:${taskId}`);
       return { event: 'task:subscribed', data: { taskId } };
     }

     @SubscribeMessage('task:typing')
     handleTyping(
       client: Socket,
       payload: { taskId: string; isTyping: boolean },
     ) {
       client.to(`task:${payload.taskId}`).emit('user:typing', {
         userId: client.data.userId,
         isTyping: payload.isTyping,
       });
     }
   }

   // Integrate with service
   @Injectable()
   export class TasksService {
     constructor(
       private tasksGateway: TasksGateway,
       private prisma: PrismaService,
     ) {}

     async createTask(data: CreateTaskDto, userId: string) {
       const task = await this.prisma.task.create({ data });
       
       // Notify in real-time
       this.tasksGateway.notifyTaskCreated(userId, task);
       
       return task;
     }

     async updateTask(id: string, updates: UpdateTaskDto) {
       const task = await this.prisma.task.update({
         where: { id },
         data: updates,
       });

       // Broadcast update
       this.tasksGateway.notifyTaskUpdated(id, task);

       return task;
     }
   }
   ```

**Deliverable:** Complete task management system with caching, background jobs, and real-time updates

---

#### Week 3: Testing Excellence & Security (40 hours)

**Day 1-2: Test-Driven Development (16 hours)**

**Goals:**
- Practice TDD workflow
- Write tests first, then implementation
- Achieve high test coverage

**TDD Cycle:**
```
1. Write failing test (RED)
2. Write minimal code to pass (GREEN)
3. Refactor (REFACTOR)
4. Repeat
```

**Example:**
```typescript
// Step 1: Write test first (RED)
describe('TaskService', () => {
  describe('assignTask', () => {
    it('should assign task to user', async () => {
      const taskId = 'task-1';
      const userId = 'user-1';

      const result = await service.assignTask(taskId, userId);

      expect(result.assigneeId).toBe(userId);
      expect(result.status).toBe(TaskStatus.IN_PROGRESS);
    });

    it('should throw if task not found', async () => {
      await expect(service.assignTask('invalid', 'user-1'))
        .rejects
        .toThrow(NotFoundException);
    });

    it('should throw if task already completed', async () => {
      // Setup completed task
      const task = await createCompletedTask();

      await expect(service.assignTask(task.id, 'user-1'))
        .rejects
        .toThrow(BadRequestException);
    });
  });
});

// Step 2: Write implementation (GREEN)
@Injectable()
export class TaskService {
  async assignTask(taskId: string, userId: string): Promise<Task> {
    const task = await this.prisma.task.findUnique({ where: { id: taskId } });
    
    if (!task) {
      throw new NotFoundException('Task not found');
    }

    if (task.status === TaskStatus.COMPLETED) {
      throw new BadRequestException('Cannot assign completed task');
    }

    return this.prisma.task.update({
      where: { id: taskId },
      data: {
        assigneeId: userId,
        status: TaskStatus.IN_PROGRESS,
      },
    });
  }
}

// Step 3: Refactor (if needed)
// Extract validation, improve naming, etc.
```

**Day 3: E2E Testing (8 hours)**

```typescript
// test/tasks.e2e-spec.ts
describe('Tasks (e2e)', () => {
  let app: INestApplication;
  let prisma: PrismaService;
  let authToken: string;

  beforeAll(async () => {
    const module = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = module.createNestApplication();
    await app.init();

    prisma = app.get(PrismaService);

    // Setup test user and get token
    const response = await request(app.getHttpServer())
      .post('/auth/login')
      .send({ email: 'test@example.com', password: 'password' });
    
    authToken = response.body.accessToken;
  });

  afterAll(async () => {
    await prisma.task.deleteMany();
    await app.close();
  });

  describe('POST /tasks', () => {
    it('should create task', () => {
      return request(app.getHttpServer())
        .post('/tasks')
        .set('Authorization', `Bearer ${authToken}`)
        .send({
          title: 'Test Task',
          description: 'Description',
        })
        .expect(201)
        .expect((res) => {
          expect(res.body).toHaveProperty('id');
          expect(res.body.title).toBe('Test Task');
        });
    });

    it('should return 401 without auth', () => {
      return request(app.getHttpServer())
        .post('/tasks')
        .send({ title: 'Test' })
        .expect(401);
    });

    it('should validate input', () => {
      return request(app.getHttpServer())
        .post('/tasks')
        .set('Authorization', `Bearer ${authToken}`)
        .send({ title: 'ab' }) // Too short
        .expect(400)
        .expect((res) => {
          expect(res.body.message).toContain('validation');
        });
    });
  });

  describe('GET /tasks', () => {
    beforeEach(async () => {
      // Seed data
      await prisma.task.createMany({
        data: [
          { title: 'Task 1', description: 'Desc 1', status: 'TODO' },
          { title: 'Task 2', description: 'Desc 2', status: 'IN_PROGRESS' },
          { title: 'Task 3', description: 'Desc 3', status: 'COMPLETED' },
        ],
      });
    });

    it('should return paginated tasks', () => {
      return request(app.getHttpServer())
        .get('/tasks?page=1&limit=2')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200)
        .expect((res) => {
          expect(res.body.data).toHaveLength(2);
          expect(res.body.meta.total).toBe(3);
          expect(res.body.meta.totalPages).toBe(2);
        });
    });

    it('should filter by status', () => {
      return request(app.getHttpServer())
        .get('/tasks?status=COMPLETED')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200)
        .expect((res) => {
          expect(res.body.data).toHaveLength(1);
          expect(res.body.data[0].status).toBe('COMPLETED');
        });
    });
  });
});
```

**Day 4: Security Hardening (8 hours)**

**Checklist:**
```typescript
// 1. Input Validation
const CreateTaskSchema = z.object({
  title: z.string().min(3).max(100).trim(),
  description: z.string().max(1000).trim(),
  dueDate: z.coerce.date().min(new Date()),
});

// 2. SQL Injection Protection (Prisma handles this)
// ✅ Prisma uses parameterized queries
await prisma.task.findMany({
  where: { title: { contains: userInput } } // Safe
});

// 3. XSS Protection
import * as sanitizeHtml from 'sanitize-html';

const sanitized = sanitizeHtml(userInput, {
  allowedTags: ['b', 'i', 'em', 'strong'],
  allowedAttributes: {},
});

// 4. Rate Limiting
@UseGuards(ThrottlerGuard)
@Throttle(10, 60) // 10 requests per 60 seconds
@Controller('auth')
export class AuthController {}

// 5. CSRF Protection
app.use(csrf());

// 6. Security Headers
import helmet from 'helmet';
app.use(helmet());

// 7. Secrets Management
// ❌ Never hardcode
const apiKey = 'sk_live_12345';

// ✅ Use environment variables
const apiKey = process.env.API_KEY;

// 8. Password Hashing
import * as bcrypt from 'bcrypt';
const hashed = await bcrypt.hash(password, 10);

// 9. JWT Best Practices
const token = jwt.sign(payload, secret, {
  expiresIn: '15m', // Short expiration
  algorithm: 'HS256',
});

// 10. HTTPS Only in Production
if (process.env.NODE_ENV === 'production') {
  app.use((req, res, next) => {
    if (!req.secure) {
      return res.redirect(`https://${req.headers.host}${req.url}`);
    }
    next();
  });
}
```

**Day 5: Performance Optimization (8 hours)**

```typescript
// 1. Database Query Optimization
// ❌ N+1 Query Problem
const tasks = await prisma.task.findMany();
for (const task of tasks) {
  task.assignee = await prisma.user.findUnique({ where: { id: task.assigneeId } });
}

// ✅ Include Relations
const tasks = await prisma.task.findMany({
  include: { assignee: true }
});

// 2. Pagination
const tasks = await prisma.task.findMany({
  skip: (page - 1) * limit,
  take: limit,
});

// 3. Select Only Needed Fields
const users = await prisma.user.findMany({
  select: { id: true, name: true, email: true }
});

// 4. Indexes
model Task {
  @@index([status])
  @@index([assigneeId])
  @@index([createdAt])
}

// 5. Caching (shown earlier)
@Cacheable(3600)
async getStats() {
  // Expensive operation
}

// 6. Lazy Loading
class User {
  @LazyLoad()
  tasks: Task[];
}

// 7. Batch Operations
await prisma.task.createMany({
  data: tasks,
  skipDuplicates: true,
});

// 8. Connection Pooling
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
  connectionLimit = 10
}

// 9. Compression
import * as compression from 'compression';
app.use(compression());

// 10. Response Caching
@Header('Cache-Control', 'max-age=3600')
@Get('public/data')
getPublicData() {}
```

**Deliverable:** Production-ready task management API with 85%+ test coverage and security hardening

---

### ✅ Phase 1 Assessment

```json
{
  "project": {
    "name": "Task Management API with Real-time Features",
    "weight": "40%",
    "requirements": [
      "Clean architecture implementation",
      "CQRS pattern for commands/queries",
      "Redis caching strategy",
      "Background job processing (Bull)",
      "Real-time updates (WebSockets)",
      "85%+ test coverage",
      "Security hardening complete",
      "Performance optimized"
    ]
  },
  "code_review": {
    "weight": "30%",
    "criteria": [
      "Architecture follows SOLID principles",
      "Proper separation of concerns",
      "Design patterns used correctly",
      "No code smells (SonarQube scan)"
    ]
  },
  "test_coverage": {
    "weight": "20%",
    "requirements": [
      "Minimum 85% overall",
      "Unit tests for all services",
      "Integration tests for all endpoints",
      "E2E tests for critical flows",
      "Tests follow TDD principles"
    ]
  },
  "security_audit": {
    "weight": "10%",
    "checklist": "OWASP Top 10",
    "requirements": [
      "No critical vulnerabilities",
      "All inputs validated",
      "Authentication secure",
      "Secrets managed properly"
    ]
  }
}
```

**Passing Score:** 85%

---

### ⏰ Time Allocation

```csv
Week,Self-Study,Coding Practice,Advanced Topics,Mentor Sessions,Code Review
1,25h,25h,20h,5h,5h
2,20h,30h,20h,5h,5h
3,20h,30h,20h,5h,5h
```

### 📖 Learning Resources

**Essential:**
- [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) - Uncle Bob
- [CQRS Pattern](https://martinfowler.com/bliki/CQRS.html) - Martin Fowler
- [Redis University](https://university.redis.com/) - Free courses
- [Bull Queue Docs](https://docs.bullmq.io/) - Background jobs
- [NestJS Microservices](https://docs.nestjs.com/microservices/basics) - Event-driven architecture

**Internal:**

---


### Weeks 4-5: System Design & User Research

**Focus:** Learn to design scalable systems based on real user needs

**Topics:**
- User research and requirements gathering
- System architecture design principles
- Scalability and performance planning
- Database design for complex domains
- API design for large systems

**Practice Project:** Design document for a complex system (e.g., multi-tenant platform)

---


## PHASE 2: Final Project Phase (Weeks 6-12)

**Objective:** Build an enterprise-grade application with full ownership

### Project Selection

At the end of Week 5, choose (with mentor guidance) ONE of these options:

#### 🏢 Option A: Internal System (Enterprise Tool)

**Best for:** Building mission-critical internal tools

**Scope:** Production-grade system used by entire company

**Example projects:**
- Complete developer onboarding platform
- Internal analytics and reporting dashboard
- Workflow automation engine
- Multi-service API gateway
- Team productivity suite

**Requirements:**
- 15-20+ comprehensive endpoints
- Multi-user with advanced RBAC
- Real-time features (WebSockets)
- Background job processing (Bull/BullMQ)
- File upload/processing (S3 integration)
- Email/notification system
- Redis caching layer
- Full audit logging
- Admin dashboard integration

**Deliverables:**
- Complete system architecture
- User research findings
- Technical specifications
- Production deployment
- 85%+ test coverage
- Monitoring and alerting
- User training materials
- Runbook and operations guide

---

#### 🌍 Option B: Real-World Application (Module Ownership)

**Best for:** Owning significant parts of production systems

**Scope:** Complete module or subsystem

**Example contributions:**
- Complete authentication/authorization system
- Payment processing module
- Advanced search functionality
- Reporting and analytics engine
- File management system
- Integration platform

**Requirements:**
- 10-15 production endpoints
- Complex business logic
- Multiple service integrations
- Performance optimization
- Comprehensive error handling
- Production monitoring
- Documentation and runbooks

**Deliverables:**
- Technical design document
- 8+ merged PRs in production
- All code reviewed and battle-tested
- 85%+ test coverage
- Performance benchmarks
- Security audit passed
- Production monitoring dashboards
- Knowledge transfer to team

---

#### 🚀 Option C: New Application (Complete Platform)

**Best for:** Building a SaaS or platform from zero

**Scope:** Full production platform ready for users

**Example platforms:**
- Multi-tenant project management system
- E-commerce marketplace backend
- Content management platform
- Social networking backend
- Real-time collaboration tool

**Requirements:**
- 20-25 comprehensive endpoints
- Advanced authentication (OAuth, 2FA)
- Multi-tenancy support
- Payment integration (Stripe/PayPal)
- File storage (S3/GCS)
- Email service integration
- Real-time features (WebSockets)
- Background jobs
- Redis caching
- Rate limiting
- API versioning
- Comprehensive logging

**Deliverables:**
- Complete system architecture
- Database with 10+ tables
- Full API documentation
- Frontend integration guide
- Production deployment (AWS/GCP)
- Custom domain + HTTPS
- CI/CD pipeline
- Monitoring and alerting
- Load testing results
- Security audit
- User documentation
- Admin panel

---

### Weeks 6-7: Discovery & Technical Design

```
Week 6: User Research & Requirements
├─ User interviews and surveys
├─ Requirements gathering
├─ Feature prioritization
├─ Use case documentation
└─ Initial wireframes/mockups

Week 7: Technical Design
├─ System architecture design
├─ Database schema design
├─ API contract definition
├─ Technology selection
├─ Security planning
└─ Deployment strategy
```

**Deliverable:** Approved technical design document

---

### Weeks 8-10: Development Sprint

```
Week 8: Core Infrastructure
├─ Project setup and CI/CD
├─ Database models and migrations
├─ Authentication/authorization
├─ Core API endpoints (30%)
└─ Unit test foundation

Weeks 9-10: Feature Development
├─ Complete all API endpoints
├─ Business logic implementation
├─ Third-party integrations
├─ Real-time features
├─ Background job processing
├─ Comprehensive testing
└─ Code reviews and refactoring
```

**Deliverable:** Feature-complete application

---

### Weeks 11-12: Production & Handover

```
Week 11: Production Deployment
├─ Deploy to production environment
├─ Set up monitoring and alerting
├─ Performance optimization
├─ Security hardening
├─ Load testing
└─ Bug fixes

Week 12: Documentation & Handover
├─ API documentation (OpenAPI/Swagger)
├─ User guide and training
├─ Operations runbook
├─ Architecture decision records
├─ Final presentation
└─ Knowledge transfer sessions
```

**Deliverable:** Production-ready system with full documentation

---

## Success Criteria

### Technical Excellence (40%)
- ✅ Clean, maintainable code following best practices
- ✅ Comprehensive testing (85%+ coverage)
- ✅ Production-grade error handling
- ✅ Security best practices implemented
- ✅ Performance optimized (load tested)
- ✅ Proper logging and monitoring

### Deliverables (30%)
- ✅ Complete working application
- ✅ Production deployment successful
- ✅ All documentation complete
- ✅ Monitoring and alerting active
- ✅ Stakeholder approval

### Professional Skills (30%)
- ✅ Independent problem solving
- ✅ Effective communication
- ✅ Time management
- ✅ Leadership and ownership
- ✅ Knowledge sharing

---

## Career Outcomes

Upon completion, you'll be ready for:

### Job Opportunities
- **Mid-Level Backend Developer** ($80k-$120k)
- **Full-Stack Developer** (if you built frontend too)
- **Technical Lead** (for smaller teams)
- **Startup Founding Engineer**

### Continued Growth
- **Senior Developer Path** - 1-2 years to senior
- **Tech Lead Path** - Lead small teams
- **Architect Path** - System design focus
- **Specialist Path** - Performance, security, etc.

See [Career Paths](./career-paths.md) for detailed roadmap.

---

## Tips for Success

### Learning Phase (Weeks 1-5)
- Build everything from scratch, don't skip
- Read open-source code daily
- Practice system design exercises
- Start project ideation early
- Document learnings continuously

### Project Phase (Weeks 6-12)
- Validate assumptions early with users
- Deploy to staging weekly
- Write tests before features (TDD)
- Keep stakeholders updated
- Don't over-engineer
- Focus on user value

---

## Navigation
[← Back to Overview](./overview.md) | [← 2-Month Program](./2-month-program.md)

**Related:** [Skills Matrix](./skills-matrix.md) | [Evaluation Criteria](./evaluation.md) | [Success Stories](./success-stories.md)
