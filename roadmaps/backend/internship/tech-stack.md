# Tech Stack Overview

This is the technology foundation you'll master during **Phase 1: Learning Phase** of your internship, then apply throughout **Phase 2: Final Project Phase**.

## Navigation
[← Back to Overview](./overview.md) | [Prerequisites →](./prerequisites.md)

---

## How This Stack Fits the Two-Phase Program

Every internship program follows this structure:

### Phase 1: Learning Phase
**Duration:** 2 weeks (1-month) | 4 weeks (2-month) | 5 weeks (3-month)

**Focus:** Master the tech stack below through:
- Structured tutorials and exercises
- Practice projects
- Code reviews from mentors
- Hands-on labs

### Phase 2: Final Project Phase
**Duration:** 2 weeks (1-month) | 4 weeks (2-month) | 7 weeks (3-month)

**Focus:** Apply everything you learned to ONE of three project types:
- 🏢 **Internal System** - Build tools for the team (admin panels, dashboards, APIs)
- 🌍 **Real-World App** - Contribute to production applications
- 🚀 **New Application** - Create your own project from scratch

**The technologies below are used in BOTH phases**—first to learn, then to build production-ready software.

---

## Stack at a Glance

| Technology | Category | Purpose | Learning Curve | Industry Adoption |
|------------|----------|---------|----------------|-------------------|
| TypeScript | Language | Type-safe JavaScript with better tooling | Moderate | Very High |
| NestJS | Framework | Enterprise-grade Node.js framework | Moderate | High |
| Zod | Validation | Runtime type checking and schema validation | Easy | Growing |
| Prisma | ORM | Type-safe database access and migrations | Easy | High |
| PostgreSQL | Database | Production-ready relational database | Moderate | Very High |

---

## How You'll Use This Stack

### During Learning Phase (Weeks 1-2/4/5)

All interns focus on:
- Building practice projects to understand each technology
- Writing tests and documentation
- Following best practices from day one
- Getting code reviews and feedback

**Example Learning Projects:**
- Todo API with authentication
- Blog API with user roles
- E-commerce product catalog

### During Final Project Phase

How this stack is applied depends on your project type:

#### 🏢 Internal System Projects

**Use case:** Build tools for internal teams
**Stack application:**
- **NestJS** - Structure enterprise features (admin panels, reporting, automation)
- **Prisma** - Model complex business data (users, permissions, analytics)
- **PostgreSQL** - Store production data safely
- **Zod** - Validate internal forms and API inputs
- **TypeScript** - Ensure code quality across modules

**Examples:**
- Developer onboarding portal (users, tasks, progress tracking)
- Analytics dashboard (data aggregation, reports, charts)
- API documentation generator (parsing, rendering, versioning)

#### 🌍 Real-World App Projects

**Use case:** Contribute features to production applications
**Stack application:**
- **NestJS** - Extend existing APIs with new endpoints
- **Prisma** - Add migrations for new features
- **PostgreSQL** - Query and update production data
- **Zod** - Validate inputs following existing patterns
- **TypeScript** - Match codebase conventions

**Examples:**
- Email notification system (events, templates, scheduling)
- Rate limiting middleware (Redis, guards, decorators)
- Invoice generation API (PDFs, templates, database queries)

#### 🚀 New Application Projects

**Use case:** Create applications from scratch
**Stack application:**
- **NestJS** - Architect the entire application structure
- **Prisma** - Design database schema from requirements
- **PostgreSQL** - Choose appropriate indexes and relations
- **Zod** - Define all validation schemas
- **TypeScript** - Write production-ready code

**Examples:**
- Task management API (projects, tasks, users, comments)
- Polling system (polls, votes, analytics, sharing)
- URL shortener (links, analytics, custom slugs)

---

## Technology Deep Dive

Below is a detailed explanation of each technology—what it is, why we chose it, and what you'll learn.

---

## TypeScript

### What is TypeScript?

TypeScript is a **strongly typed superset of JavaScript** that compiles to plain JavaScript. Think of it as JavaScript with superpowers—you get all the flexibility of JavaScript plus the safety and tooling benefits of static typing.

### Why TypeScript?

**Benefits:**

| Benefit | Description | Example | Impact |
|---------|-------------|---------|--------|
| **Type Safety** | Catch errors at compile time, not runtime | Prevents passing a string where a number is expected | Reduces bugs by 15-30% according to industry studies |
| **Better Tooling** | IntelliSense, autocomplete, refactoring support | IDE shows all available methods on an object | Developers are 20-40% more productive |
| **Self-Documenting** | Types serve as inline documentation | Function signatures tell you exactly what to pass | Reduces time spent reading documentation |
| **Scalability** | Easier to maintain large codebases | Refactoring across 100 files? TypeScript catches all issues | Essential for teams and long-term projects |

### Key Features You'll Learn

#### 1. Basic Types
```typescript
// Primitives
const age: number = 25;
const name: string = "John";
const isActive: boolean = true;

// Arrays
const numbers: number[] = [1, 2, 3];
const users: string[] = ["Alice", "Bob"];
```

#### 2. Interfaces and Types
```typescript
// Define shape of objects
interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user'; // Union type
}

// Use it
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  role: "admin"
};
```

#### 3. Generics
```typescript
// Reusable, type-safe functions
function getFirst<T>(items: T[]): T | undefined {
  return items[0];
}

const firstNumber = getFirst([1, 2, 3]); // number | undefined
const firstString = getFirst(["a", "b"]); // string | undefined
```

### When You'll Use It

- **Every day** - All your code will be in TypeScript
- **Week 1** - Learning fundamentals
- **Week 2+** - Advanced patterns like generics, utility types

### Learning Resources

- [TypeScript Official Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) - Comprehensive guide
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/) - Free online book
- [Type Challenges](https://github.com/type-challenges/type-challenges) - Practice exercises

---

## NestJS

### What is NestJS?

NestJS is a **progressive Node.js framework** for building efficient, scalable server-side applications. It uses TypeScript by default and is heavily inspired by Angular's architecture.

### Why NestJS?

| Criteria | NestJS | Express (Alternative) | Fastify (Alternative) |
|----------|--------|----------------------|----------------------|
| **Architecture** | Opinionated, modular | Minimal, flexible | Minimal, flexible |
| **TypeScript** | First-class support | Needs setup | Needs setup |
| **Enterprise-ready** | Yes (built-in patterns) | Requires custom setup | Requires custom setup |
| **Learning curve** | Moderate | Easy | Easy |
| **Scalability** | Excellent | Good (manual) | Good (manual) |
| **Best for** | Teams, long-term projects | Quick prototypes | Performance-critical apps |

**Why we chose NestJS:**
1. **Enforces best practices** - You learn proper architecture from day one
2. **Everything included** - Validation, testing, ORM integration, authentication
3. **Industry adoption** - Used by companies like Adidas, Capgemini, Roche
4. **Great documentation** - Comprehensive guides and examples

### Core Concepts

#### 1. Modules - Organize Your Code

```typescript
// users.module.ts
@Module({
  imports: [DatabaseModule], // Other modules this module depends on
  controllers: [UsersController], // HTTP request handlers
  providers: [UsersService], // Business logic
  exports: [UsersService], // Make service available to other modules
})
export class UsersModule {}
```

**Think of modules as self-contained features** - everything related to "users" lives in the Users module.

#### 2. Controllers - Handle HTTP Requests

```typescript
// users.controller.ts
@Controller('users') // Base route: /users
export class UsersController {
  constructor(private usersService: UsersService) {} // Dependency injection

  @Get() // GET /users
  findAll() {
    return this.usersService.findAll();
  }

  @Post() // POST /users
  create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }
}
```

#### 3. Services - Business Logic

```typescript
// users.service.ts
@Injectable() // Makes it available for dependency injection
export class UsersService {
  constructor(private prisma: PrismaService) {}

  async findAll() {
    return this.prisma.user.findMany();
  }

  async create(data: CreateUserDto) {
    return this.prisma.user.create({ data });
  }
}
```

#### 4. Dependency Injection - The Magic

```typescript
// NestJS automatically provides dependencies
@Controller('users')
export class UsersController {
  // No need to manually create UsersService
  // NestJS injects it automatically
  constructor(private usersService: UsersService) {}
}
```

**Benefits:**
- Easy to test (inject mocks)
- Loose coupling
- Single responsibility

### When You'll Use It

- **Week 1** - Learn modules, controllers, services
- **Week 2** - Guards, pipes, interceptors
- **Throughout** - Your entire backend is built with NestJS

### Learning Resources

- [NestJS Official Docs](https://docs.nestjs.com/) - Excellent documentation
- [NestJS Fundamentals Course](https://learn.nestjs.com/) - Official free course

---

## Zod

### What is Zod?

Zod is a **TypeScript-first schema validation library**. It provides both runtime validation and compile-time type inference.

### Why Zod?

**The Problem:** TypeScript only checks types at compile time. At runtime, any invalid data can slip through.

```typescript
// TypeScript can't catch this!
interface User {
  email: string;
}

// This passes TypeScript, but email is invalid
const user: User = JSON.parse('{"email": "not-an-email"}');
```

**The Solution:** Zod validates at runtime:

```typescript
import { z } from 'zod';

// Define schema
const UserSchema = z.object({
  email: z.string().email(), // Validates email format
  age: z.number().min(18), // Must be 18+
});

// Validate data
const result = UserSchema.safeParse({
  email: "not-an-email",
  age: 15
});

if (!result.success) {
  console.log(result.error); // Shows all validation errors
}
```

### Key Features

#### 1. Schema Definition

```typescript
const CreateUserSchema = z.object({
  name: z.string().min(2).max(50),
  email: z.string().email(),
  age: z.number().int().positive().min(18).max(120),
  role: z.enum(['admin', 'user']),
  bio: z.string().optional(), // Optional field
  tags: z.array(z.string()), // Array of strings
});
```

#### 2. Type Inference

```typescript
// Zod automatically creates TypeScript type!
type CreateUserDto = z.infer<typeof CreateUserSchema>;

// Equivalent to:
// type CreateUserDto = {
//   name: string;
//   email: string;
//   age: number;
//   role: 'admin' | 'user';
//   bio?: string;
//   tags: string[];
// }
```

#### 3. Custom Validation

```typescript
const PasswordSchema = z
  .string()
  .min(8)
  .refine((val) => /[A-Z]/.test(val), "Must contain uppercase")
  .refine((val) => /[0-9]/.test(val), "Must contain number");
```

### Alternatives Comparison

| Library | Type Safety | Runtime Check | DX | Bundle Size | Best For |
|---------|-------------|---------------|-----|-------------|----------|
| Zod | Excellent | Yes | Excellent | Small | TypeScript projects |
| Joi | None | Yes | Good | Medium | JavaScript projects |
| Yup | Basic | Yes | Good | Medium | Form validation |
| class-validator | Good | Yes | Good | Large | NestJS (traditional approach) |

**Why Zod wins for us:**
- Perfect TypeScript integration
- Compose schemas easily
- Great error messages
- Lightweight

### When You'll Use It

- **Week 2** - Learn schema creation
- **Every API endpoint** - Validate all incoming data
- **Database operations** - Ensure data integrity

### Learning Resources

- [Zod Official Docs](https://zod.dev/) - Comprehensive guide
- [Zod + NestJS Integration](https://docs.nestjs.com/techniques/validation#auto-validation)

---

## Prisma

### What is Prisma?

Prisma is a **next-generation ORM** (Object-Relational Mapping) that makes database access type-safe and enjoyable.

### Why Prisma?

**Traditional ORM problems:**
- Not type-safe (easy to make mistakes)
- Complex query builders
- Migration headaches
- Poor TypeScript support

**Why Prisma solutions:**

| Benefit | Description | Impact |
|---------|-------------|--------|
| **Type Safety** | Auto-generated TypeScript types from schema | Impossible to write invalid queries |
| **Intuitive API** | Clean, readable query syntax | Write queries faster, easier to maintain |
| **Migrations** | Declarative migrations from schema changes | No manual SQL for schema changes |
| **Prisma Studio** | Visual database browser | Debug and manage data without SQL |
| **Performance** | Optimized queries, connection pooling | Production-ready performance |

### Core Concepts

#### 1. Prisma Schema

Define your database structure in one file:

```prisma
// schema.prisma
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  posts     Post[]   // Relation to Post
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

#### 2. Prisma Client - Query Database

```typescript
// Auto-generated from schema above
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

// Create user with posts
const user = await prisma.user.create({
  data: {
    email: 'alice@example.com',
    name: 'Alice',
    posts: {
      create: [
        { title: 'Hello World', content: 'My first post' }
      ]
    }
  },
  include: { posts: true } // Include relations
});

// Find users with filters
const users = await prisma.user.findMany({
  where: {
    email: { contains: '@example.com' }
  },
  orderBy: { createdAt: 'desc' },
  take: 10 // Limit to 10
});

// Update
await prisma.user.update({
  where: { id: 1 },
  data: { name: 'Alice Updated' }
});

// Delete
await prisma.user.delete({
  where: { id: 1 }
});
```

**Notice:** Fully type-safe! Your IDE knows all fields, relations, and methods.

#### 3. Migrations

```bash
# Create migration from schema changes
npx prisma migrate dev --name add-user-role

# Apply migrations in production
npx prisma migrate deploy
```

Prisma automatically generates SQL migrations when you change your schema.

### Alternatives Comparison

| Feature | Prisma | TypeORM | Sequelize |
|---------|--------|---------|-----------|
| TypeScript Support | Excellent | Good | Basic |
| Type Safety | 100% | ~80% | ~40% |
| Query API | Intuitive | Complex | Verbose |
| Migrations | Declarative | Code-based | Mixed |
| Relations | Easy | Moderate | Complex |
| Learning Curve | Easy | Moderate | Moderate |

### When You'll Use It

- **Week 2** - Learn schema, migrations, queries
- **Every database operation** - All CRUD operations
- **Throughout** - Your primary database tool

### Learning Resources

- [Prisma Official Docs](https://www.prisma.io/docs) - Excellent guides
- [Prisma + NestJS Guide](https://docs.nestjs.com/recipes/prisma)

---

## PostgreSQL

### What is PostgreSQL?

PostgreSQL (often called "Postgres") is a **powerful, open-source relational database** known for reliability, feature robustness, and performance.

### Why PostgreSQL?

| Database | Type | Best For | Pros | Cons | Adoption |
|----------|------|----------|------|------|----------|
| PostgreSQL | Relational | Complex queries, ACID transactions | Feature-rich, Reliable, Open-source | Slightly slower than MySQL | Very High |
| MySQL | Relational | Read-heavy workloads | Fast, Simple, Popular | Limited features | Very High |
| MongoDB | Document | Flexible schemas, Rapid development | Schema flexibility, Horizontal scaling | No ACID (by default) | High |
| Redis | Key-Value | Caching, Sessions | Extremely fast, In-memory | Not persistent (primary) | High |

**Why PostgreSQL wins:**

1. **ACID Compliant** - Your data is safe, transactions are reliable
2. **Advanced Features**:
   - JSON support (best of both worlds: relational + document)
   - Full-text search
   - Array and custom types
   - Powerful indexing
3. **Industry Standard** - Used by Apple, Instagram, Reddit, Uber
4. **Great Prisma Integration** - Prisma works best with Postgres

### Key Features You'll Use

#### 1. Data Types

```sql
-- Rich type system
id          SERIAL PRIMARY KEY
name        VARCHAR(255)
email       VARCHAR(255) UNIQUE
age         INTEGER
salary      DECIMAL(10, 2)
is_active   BOOLEAN
tags        TEXT[]              -- Arrays!
metadata    JSONB               -- JSON with indexing
created_at  TIMESTAMP DEFAULT NOW()
```

#### 2. Relationships

```sql
-- One-to-Many
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255)
);

CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  title VARCHAR(255),
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
);
```

#### 3. Indexes for Performance

```sql
-- Speed up queries
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_created_at ON posts(created_at DESC);
```

#### 4. Transactions

```sql
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- Both succeed or both fail
```

### PostgreSQL vs Others for This Program

**Why not MySQL?**
- PostgreSQL has better JSON support
- More advanced features (arrays, custom types)
- Better standards compliance

**Why not MongoDB?**
- Learning relational databases is fundamental
- Most production systems use SQL
- ACID transactions are critical for business logic

**Why not SQLite?**
- Not suitable for production at scale
- Limited concurrency
- Used for development/testing only

### When You'll Use It

- **Week 1** - Setup local database
- **Week 2** - Learn queries, joins, indexes
- **Throughout** - Via Prisma (you won't write much raw SQL)

### What You'll Learn

**Beginner Level:**
- Basic CRUD operations
- Simple queries (SELECT, WHERE, ORDER BY)
- Primary keys and foreign keys
- Basic joins (INNER JOIN, LEFT JOIN)

**Intermediate Level:**
- Indexes for performance
- Transactions
- Aggregations (COUNT, SUM, AVG)
- Subqueries

**Advanced Level:**
- Query optimization
- JSONB operations
- Full-text search
- Database design for scale

### Learning Resources

- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/) - Great for beginners
- [PostgreSQL Official Docs](https://www.postgresql.org/docs/) - Reference

---

## How Everything Works Together

### Request Flow

```
1. Client sends HTTP request
   ↓
2. NestJS Controller receives request
   ↓
3. Zod validates request body
   ↓
4. Controller calls Service
   ↓
5. Service uses Prisma to query database
   ↓
6. PostgreSQL executes query
   ↓
7. Prisma returns type-safe data
   ↓
8. Service processes and returns data
   ↓
9. Controller sends response
```

### Code Example - Complete Flow

```typescript
// 1. Zod Schema (validation)
const CreatePostSchema = z.object({
  title: z.string().min(5),
  content: z.string(),
});

type CreatePostDto = z.infer<typeof CreatePostSchema>;

// 2. NestJS Controller
@Controller('posts')
export class PostsController {
  constructor(private postsService: PostsService) {}

  @Post()
  async create(@Body() body: CreatePostDto) {
    // Zod validation happens automatically (via pipe)
    return this.postsService.create(body);
  }
}

// 3. NestJS Service
@Injectable()
export class PostsService {
  constructor(private prisma: PrismaService) {}

  async create(data: CreatePostDto) {
    // 4. Prisma queries PostgreSQL
    return this.prisma.post.create({
      data: {
        title: data.title,
        content: data.content,
      },
    });
  }
}
```

### Why This Stack is Beginner-Friendly

| Aspect | Why It Helps |
|--------|--------------|
| **TypeScript** | Catches mistakes as you type |
| **NestJS** | Enforces structure, prevents messy code |
| **Zod** | Clear validation errors help debugging |
| **Prisma** | Type-safe queries prevent SQL injection, runtime errors |
| **PostgreSQL** | Industry standard with tons of resources |

### Industry Relevance

Companies using this stack (or similar):
- **Adidas** - NestJS, TypeScript
- **Roche** - NestJS for healthcare APIs
- **Tripadvisor** - Node.js, TypeScript, PostgreSQL
- **Netflix** - Node.js, TypeScript
- **Uber** - Node.js, PostgreSQL

**This is not a toy stack—this is production-grade technology.**

---

## What's Not Included (And Why)

| Technology | Why Not Included | When to Learn |
|------------|------------------|---------------|
| **React/Vue** | Focus on backend first | After mastering backend |
| **GraphQL** | REST is more fundamental | After REST mastery |
| **Microservices** | Too complex for beginners | After monolith experience |
| **Docker** | Included in 2-3 month programs | Week 7+ |
| **Kubernetes** | Advanced DevOps | After 6+ months experience |

---

## Summary: Your Learning Path

### Week 1
- **TypeScript**: Basic types, interfaces
- **NestJS**: Modules, controllers, services
- **Setup**: Install everything, run first app

### Week 2
- **Zod**: Schema validation
- **Prisma**: Database modeling, migrations
- **PostgreSQL**: Basic queries

### Week 3+
- **Integration**: All technologies working together
- **Advanced Patterns**: Guards, interceptors, transactions
- **Real Projects**: Production-ready features

---

## Next Steps

Ready to get started? Check your prerequisites:

→ **[Prerequisites Guide](./prerequisites.md)** - Setup instructions

Or jump into a program:
- **[1-Month Program](./1-month-program.md)**
- **[2-Month Program](./2-month-program.md)**
- **[3-Month Program](./3-month-program.md)**

---

## Questions?

See **[FAQs](./faqs.md)** for common tech stack questions, or ask your mentor during onboarding.

---

[← Back to Overview](./overview.md) | [Prerequisites →](./prerequisites.md)
