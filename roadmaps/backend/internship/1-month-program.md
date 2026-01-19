---
id: 1-month-program
---

# 1-Month Internship Program

**Objective**: Learn backend fundamentals quickly and apply them through a final project.

## Navigation
[← Back to Overview](./overview.md) | [Next: 2-Month Program →](./2-month-program.md)

**Related:** [Tech Stack](./tech-stack.md) | [Prerequisites](./prerequisites.md) | [Skills Matrix](./skills-matrix.md)

---

## Program Structure

This 1-month program follows a **two-phase approach**:

```
┌─────────────────────────────────────────────────────┐
│           1-MONTH INTERNSHIP PROGRAM                │
├─────────────────────────────────────────────────────┤
│                                                     │
│  PHASE 1: LEARNING (Weeks 1-2)                     │
│  ├─ Week 1: TypeScript + NestJS Fundamentals       │
│  └─ Week 2: Database, APIs, Testing                │
│                                                     │
│  PHASE 2: FINAL PROJECT (Weeks 3-4)                │
│  Choose ONE option:                                 │
│  ├─ 🏢 Internal System (Build internal tool)       │
│  ├─ 🌍 Real-World App (Join production project)    │
│  └─ 🚀 New Application (Create from scratch)       │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### What You'll Achieve

By the end of this program:
- ✅ Master TypeScript and NestJS fundamentals
- ✅ Build CRUD APIs with Prisma and PostgreSQL
- ✅ Write tests and validate data with Zod
- ✅ Complete a real project (3+ merged PRs or deployed feature)
- ✅ Be **ready for a junior backend role**

---

## PHASE 1: Learning Phase (Weeks 1-2)

**Objective:** Master the tech stack fundamentals

## Week 1: Foundations & Environment Setup

### 🎯 Learning Objectives / Competencies

By the end of Week 1, you will be able to:
- Set up a complete TypeScript/NestJS development environment
- Understand and use TypeScript's type system confidently
- Explain NestJS architecture (modules, controllers, services)
- Use Git for version control and collaboration
- Run the company's production codebase locally
- Navigate and understand the codebase structure

### 📚 Core Materials

```csv
Topic,Subtopic,Priority,Time Required,Assessment Method
TypeScript,Basic types and interfaces,High,4 hours,Coding exercises
TypeScript,Generics and utility types,Medium,3 hours,Code review
NestJS,Modules Controllers Services,High,5 hours,Build sample API
NestJS,Dependency Injection,High,3 hours,Explain to mentor
Git,Branching and Pull Requests,High,2 hours,Create sample PR
Tooling,VS Code setup ESLint Prettier,Medium,1 hour,Show configured editor
```

### 🔨 Learning Activities

#### Day 1-2: TypeScript Fundamentals (16 hours)

**Goals:**
- Master TypeScript basics
- Build confidence with type system
- Create a small project

**Activities:**

1. **Self-Study (8 hours)**
   - Read [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) sections:
     - Basic Types
     - Interfaces
     - Functions
     - Generics
   - Watch: [TypeScript Crash Course](https://www.youtube.com/watch?v=BCg4U1FzODs) (90 minutes)

2. **Hands-On Project (6 hours)**
   - Build a CLI tool: **Task Manager**
   - Features:
     ```typescript
     // Define types
     interface Task {
       id: string;
       title: string;
       completed: boolean;
       createdAt: Date;
     }

     // Implement functions
     function addTask(title: string): Task
     function listTasks(): Task[]
     function completeTask(id: string): void
     function deleteTask(id: string): void
     ```
   - Use `fs` module to persist data as JSON
   - Practice: interfaces, types, generics, error handling

3. **Practice Exercises (2 hours)**
   - [Type Challenges - Easy](https://github.com/type-challenges/type-challenges) (complete 5 challenges)
   - Focus on: Pick, Omit, Readonly, ReturnType

**Deliverable:** CLI task manager + 5 Type Challenge solutions

---

#### Day 3-4: NestJS Introduction (16 hours)

**Goals:**
- Understand NestJS architecture
- Build first REST API
- Master dependency injection

**Activities:**

1. **Official Course (6 hours)**
   - Complete [NestJS Fundamentals](https://learn.nestjs.com/) course
   - Topics covered:
     - Controllers and routing
     - Providers and services
     - Modules
     - Dependency injection
     - Pipes and validation

2. **Build: CRUD API (8 hours)**
   - Project: **Book Library API**
   - Endpoints:
     ```
     GET    /books           - List all books
     GET    /books/:id       - Get book by ID
     POST   /books           - Create book
     PATCH  /books/:id       - Update book
     DELETE /books/:id       - Delete book
     ```
   - Store data in-memory (array) for now
   - Practice:
     ```typescript
     // books.module.ts
     @Module({
       controllers: [BooksController],
       providers: [BooksService],
     })
     export class BooksModule {}

     // books.controller.ts
     @Controller('books')
     export class BooksController {
       constructor(private booksService: BooksService) {}

       @Get()
       findAll() {
         return this.booksService.findAll();
       }
       
       // ... implement other endpoints
     }

     // books.service.ts
     @Injectable()
     export class BooksService {
       private books: Book[] = [];

       findAll() {
         return this.books;
       }

       // ... implement CRUD logic
     }
     ```

3. **Testing (2 hours)**
   - Write basic unit tests for service
   - Run tests with `npm test`
   - Achieve 70%+ coverage

**Deliverable:** Working Book Library API with tests

---

#### Day 5: Development Workflow & Codebase Onboarding (8 hours)

**Goals:**
- Set up company project locally
- Understand codebase architecture
- Learn team's Git workflow

**Activities:**

1. **Environment Setup (2 hours)**
   - Clone company repository
   - Install dependencies
   - Configure environment variables
   - Set up database (if needed)
   - Run project locally
   - Verify all tests pass

2. **Codebase Exploration (4 hours)**
   - **Guided tour with mentor (1 hour)**:
     - Project structure overview
     - Key modules and their responsibilities
     - Where to find documentation
     - How to run tests and builds
   
   - **Self-exploration (3 hours)**:
     - Read README and contributing guide
     - Trace through one complete request flow
     - Identify patterns (error handling, validation, etc.)
     - Note questions for mentor

3. **Git Workflow Practice (2 hours)**
   - Review team's branching strategy (Git Flow, GitHub Flow, etc.)
   - Create feature branch
   - Make small change (fix typo in comments)
   - Submit pull request
   - Practice responding to review feedback

**Deliverable:** Project running locally + understanding of codebase structure

---

### ✅ Week 1 Assessment

To successfully complete Week 1, you must pass all assessments:

#### 1. Code Review: TypeScript CLI Tool (Weight: 25%)

**Criteria:**
- [ ] Correct TypeScript types (no `any` unless justified)
- [ ] Proper error handling
- [ ] Clean, readable code
- [ ] Works as specified

**Passing Score:** 70%

---

#### 2. Quiz: NestJS Architecture Concepts (Weight: 25%)

**Sample Questions:**
1. What is dependency injection and why is it useful?
2. Explain the role of modules, controllers, and providers.
3. How does NestJS handle HTTP requests? (request lifecycle)
4. What are decorators and how are they used in NestJS?

**Format:** 10 questions, multiple choice + short answer
**Passing Score:** 80%
**Time:** 30 minutes

---

#### 3. Practical: Run Company Project Locally (Weight: 30%)

**Checklist:**
- [ ] Repository cloned
- [ ] Dependencies installed
- [ ] Environment configured
- [ ] Database running (if applicable)
- [ ] Application starts without errors
- [ ] All existing tests pass
- [ ] Able to make API requests

**Evaluation:** Pass/Fail (must complete 100%)

---

#### 4. Presentation: Codebase Structure Walkthrough (Weight: 20%)

**Requirements:**
- 10-minute presentation to mentor
- Cover:
  - High-level architecture
  - Key modules and their purposes
  - How a typical request is handled
  - Where tests live
  - Deployment process (if known)
  
**Evaluation Rubric:**
```json
{
  "clarity": "Can you explain concepts clearly?",
  "depth": "Do you understand beyond surface level?",
  "curiosity": "Did you explore beyond requirements?",
  "questions": "Are your questions thoughtful?"
}
```

**Passing Score:** 3/4 criteria met

---

### ⏰ Time Allocation

| Activity | Hours | Percentage |
|----------|-------|------------|
| Self-study | 20 | 50% |
| Coding practice | 15 | 38% |
| Mentor sessions | 3 | 7% |
| Code review | 2 | 5% |
| **Total** | **40** | **100%** |

### 📖 Learning Resources

#### Essential (Must Read/Watch)
- [TypeScript Official Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [NestJS Official Documentation](https://docs.nestjs.com/)
- [NestJS Fundamentals Course](https://learn.nestjs.com/) (Free)

#### Supplementary
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/) - Free book
- [Understanding Dependency Injection](https://blog.logrocket.com/understanding-dependency-injection-in-nestjs/)
- [Git Branching Strategies](https://www.atlassian.com/git/tutorials/comparing-workflows)

#### Internal Resources
- Company internal documentation

---

## Week 2: Core Backend Concepts

### 🎯 Learning Objectives / Competencies

By the end of Week 2, you will be able to:
- Design and implement database schemas with Prisma
- Run and manage database migrations
- Validate incoming data using Zod
- Write unit and integration tests
- Handle errors gracefully
- Query PostgreSQL effectively

### 📚 Core Materials

```json
{
  "modules": [
    {
      "name": "Prisma ORM",
      "topics": ["Schema definition", "Migrations", "CRUD operations", "Relations"],
      "priority": "High",
      "hours": 8,
      "resources": ["https://www.prisma.io/docs", "Company DB patterns"]
    },
    {
      "name": "Zod Validation",
      "topics": ["Schema creation", "Type inference", "Error handling", "Custom validators"],
      "priority": "High",
      "hours": 4,
      "resources": ["https://zod.dev/", "NestJS Zod integration"]
    },
    {
      "name": "PostgreSQL",
      "topics": ["Basic queries", "Indexes", "Transactions", "Query optimization"],
      "priority": "Medium",
      "hours": 4,
      "resources": ["https://www.postgresqltutorial.com/"]
    },
    {
      "name": "Testing",
      "topics": ["Jest basics", "Unit tests", "Integration tests", "Mocking"],
      "priority": "High",
      "hours": 6,
      "resources": ["https://docs.nestjs.com/fundamentals/testing"]
    }
  ]
}
```

### 🔨 Learning Activities

#### Day 1-2: Database & ORM (16 hours)

**Goals:**
- Master Prisma schema definition
- Understand database relationships
- Practice migrations

**Activities:**

1. **PostgreSQL Setup (1 hour)**
   ```bash
   # Install PostgreSQL locally
   # macOS: brew install postgresql
   # Ubuntu: sudo apt install postgresql
   
   # Create database
   createdb blog_dev
   
   # Verify connection
   psql blog_dev
   ```

2. **Prisma Fundamentals (4 hours)**
   - Read [Prisma Getting Started](https://www.prisma.io/docs/getting-started)
   - Learn:
     - Schema syntax
     - Data modeling
     - Relationships (1-to-1, 1-to-many, many-to-many)
     - Migrations workflow

3. **Build: Blog System Database (8 hours)**
   
   **Define Schema:**
   ```prisma
   // schema.prisma
   model User {
     id        Int       @id @default(autoincrement())
     email     String    @unique
     name      String
     password  String    // Hashed
     role      Role      @default(USER)
     posts     Post[]
     comments  Comment[]
     createdAt DateTime  @default(now())
     updatedAt DateTime  @updatedAt
   }

   model Post {
     id        Int       @id @default(autoincrement())
     title     String
     content   String?
     published Boolean   @default(false)
     author    User      @relation(fields: [authorId], references: [id])
     authorId  Int
     comments  Comment[]
     tags      Tag[]
     createdAt DateTime  @default(now())
     updatedAt DateTime  @updatedAt

     @@index([authorId])
     @@index([published])
   }

   model Comment {
     id        Int      @id @default(autoincrement())
     content   String
     author    User     @relation(fields: [authorId], references: [id])
     authorId  Int
     post      Post     @relation(fields: [postId], references: [id])
     postId    Int
     createdAt DateTime @default(now())

     @@index([postId])
   }

   model Tag {
     id    Int    @id @default(autoincrement())
     name  String @unique
     posts Post[]
   }

   enum Role {
     USER
     ADMIN
   }
   ```

   **Practice Queries:**
   ```typescript
   // Create user with posts
   const user = await prisma.user.create({
     data: {
       email: 'alice@example.com',
       name: 'Alice',
       password: 'hashed_password',
       posts: {
         create: [
           { 
             title: 'First Post', 
             content: 'Hello World',
             tags: {
               connectOrCreate: [
                 { where: { name: 'NestJS' }, create: { name: 'NestJS' } }
               ]
             }
           }
         ]
       }
     },
     include: { posts: true }
   });

   // Complex query with filters
   const posts = await prisma.post.findMany({
     where: {
       published: true,
       tags: {
         some: { name: 'NestJS' }
       }
     },
     include: {
       author: { select: { name: true, email: true } },
       comments: { take: 5, orderBy: { createdAt: 'desc' } },
       tags: true
     },
     orderBy: { createdAt: 'desc' },
     take: 10
   });

   // Update with relations
   await prisma.post.update({
     where: { id: 1 },
     data: {
       published: true,
       tags: {
         connect: [{ name: 'TypeScript' }]
       }
     }
   });
   ```

4. **Seed Data (1 hour)**
   ```typescript
   // prisma/seed.ts
   async function main() {
     // Create users
     const alice = await prisma.user.create({
       data: { email: 'alice@test.com', name: 'Alice', password: 'hash' }
     });

     // Create posts with relations
     // ... seed 20-30 records
   }
   ```

5. **Explore with Prisma Studio (2 hours)**
   ```bash
   npx prisma studio
   ```
   - Browse data visually
   - Test CRUD operations
   - Understand relations

**Deliverable:** Complete blog database schema + seed data

---

#### Day 3-4: Validation & Error Handling (16 hours)

**Goals:**
- Master Zod validation
- Implement robust error handling
- Create custom validation pipes

**Activities:**

1. **Zod Deep Dive (3 hours)**
   - Read [Zod Documentation](https://zod.dev/)
   - Practice:
     ```typescript
     import { z } from 'zod';

     // Basic schemas
     const UserSchema = z.object({
       email: z.string().email(),
       name: z.string().min(2).max(50),
       age: z.number().int().positive().min(18).optional(),
     });

     // Nested schemas
     const CreatePostSchema = z.object({
       title: z.string().min(5).max(200),
       content: z.string().min(10),
       tags: z.array(z.string()).min(1).max(5),
       published: z.boolean().default(false),
     });

     // Type inference
     type CreatePostDto = z.infer<typeof CreatePostSchema>;

     // Custom validation
     const PasswordSchema = z.string()
       .min(8)
       .refine(v => /[A-Z]/.test(v), "Need uppercase")
       .refine(v => /[0-9]/.test(v), "Need number")
       .refine(v => /[^A-Za-z0-9]/.test(v), "Need special char");
     ```

2. **Integrate Zod with NestJS (4 hours)**
   ```typescript
   // Create Zod validation pipe
   import { PipeTransform, BadRequestException } from '@nestjs/common';
   import { ZodSchema } from 'zod';

   export class ZodValidationPipe implements PipeTransform {
     constructor(private schema: ZodSchema) {}

     transform(value: unknown) {
       const result = this.schema.safeParse(value);
       if (!result.success) {
         throw new BadRequestException({
           message: 'Validation failed',
           errors: result.error.format()
         });
       }
       return result.data;
     }
   }

   // Use in controller
   @Post()
   create(
     @Body(new ZodValidationPipe(CreatePostSchema)) 
     body: CreatePostDto
   ) {
     return this.postsService.create(body);
   }
   ```

3. **Error Handling Strategy (3 hours)**
   ```typescript
   // Custom exception filter
   @Catch()
   export class GlobalExceptionFilter implements ExceptionFilter {
     catch(exception: unknown, host: ArgumentsHost) {
       const ctx = host.switchToHttp();
       const response = ctx.getResponse();
       const request = ctx.getRequest();

       // Handle different error types
       if (exception instanceof PrismaClientKnownRequestError) {
         // Database errors
         if (exception.code === 'P2002') {
           return response.status(409).json({
             message: 'Unique constraint violation',
             field: exception.meta?.target
           });
         }
       }

       if (exception instanceof ZodError) {
         return response.status(400).json({
           message: 'Validation error',
           errors: exception.errors
         });
       }

       // Default error response
       response.status(500).json({
         message: 'Internal server error',
         timestamp: new Date().toISOString(),
         path: request.url
       });
     }
   }
   ```

4. **Build Complete Validated API (6 hours)**
   - Extend blog API with validation
   - Handle all edge cases
   - Return meaningful error messages

**Deliverable:** Blog API with comprehensive validation and error handling

---

#### Day 5: Testing Fundamentals (8 hours)

**Goals:**
- Write effective unit tests
- Create integration tests
- Mock dependencies
- Achieve 70%+ coverage

**Activities:**

1. **Jest Basics (2 hours)**
   - Review [Jest Documentation](https://jestjs.io/docs/getting-started)
   - Understand:
     - Test structure (describe, it, expect)
     - Matchers (toBe, toEqual, toThrow)
     - Setup and teardown
     - Mocking

2. **Unit Tests for Services (3 hours)**
   ```typescript
   // posts.service.spec.ts
   describe('PostsService', () => {
     let service: PostsService;
     let prisma: PrismaService;

     beforeEach(async () => {
       const module = await Test.createTestingModule({
         providers: [
           PostsService,
           {
             provide: PrismaService,
             useValue: {
               post: {
                 findMany: jest.fn(),
                 findUnique: jest.fn(),
                 create: jest.fn(),
                 update: jest.fn(),
                 delete: jest.fn(),
               }
             }
           }
         ],
       }).compile();

       service = module.get<PostsService>(PostsService);
       prisma = module.get<PrismaService>(PrismaService);
     });

     describe('findAll', () => {
       it('should return array of posts', async () => {
         const mockPosts = [{ id: 1, title: 'Test' }];
         jest.spyOn(prisma.post, 'findMany').mockResolvedValue(mockPosts);

         const result = await service.findAll();
         
         expect(result).toEqual(mockPosts);
         expect(prisma.post.findMany).toHaveBeenCalledTimes(1);
       });
     });

     describe('create', () => {
       it('should create a post', async () => {
         const createDto = { title: 'New Post', content: 'Content' };
         const mockPost = { id: 1, ...createDto };
         
         jest.spyOn(prisma.post, 'create').mockResolvedValue(mockPost);

         const result = await service.create(createDto);
         
         expect(result).toEqual(mockPost);
         expect(prisma.post.create).toHaveBeenCalledWith({ data: createDto });
       });

       it('should throw error if validation fails', async () => {
         const invalidDto = { title: 'ab' }; // Too short
         
         await expect(service.create(invalidDto)).rejects.toThrow();
       });
     });
   });
   ```

3. **Integration Tests for Controllers (3 hours)**
   ```typescript
   // posts.controller.spec.ts (e2e)
   describe('PostsController (e2e)', () => {
     let app: INestApplication;

     beforeAll(async () => {
       const module = await Test.createTestingModule({
         imports: [AppModule],
       }).compile();

       app = module.createNestApplication();
       await app.init();
     });

     afterAll(async () => {
       await app.close();
     });

     describe('/posts (GET)', () => {
       it('should return 200 and array of posts', () => {
         return request(app.getHttpServer())
           .get('/posts')
           .expect(200)
           .expect((res) => {
             expect(Array.isArray(res.body)).toBe(true);
           });
       });
     });

     describe('/posts (POST)', () => {
       it('should create post with valid data', () => {
         return request(app.getHttpServer())
           .post('/posts')
           .send({ title: 'Test Post', content: 'Content' })
           .expect(201)
           .expect((res) => {
             expect(res.body).toHaveProperty('id');
             expect(res.body.title).toBe('Test Post');
           });
       });

       it('should return 400 with invalid data', () => {
         return request(app.getHttpServer())
           .post('/posts')
           .send({ title: 'ab' }) // Too short
           .expect(400);
       });
     });
   });
   ```

**Deliverable:** Test suite with 70%+ coverage

---

### ✅ Week 2 Assessment

#### 1. Project: Mini Blog API (Weight: 40%)

**Requirements:**
- [ ] Users can register and login
- [ ] Authenticated users can create posts
- [ ] Anyone can view published posts
- [ ] Authors can comment on any post
- [ ] Posts can have multiple tags
- [ ] Full Zod validation on all endpoints
- [ ] Comprehensive error handling
- [ ] 70%+ test coverage

**Endpoints Required:**
```
POST   /auth/register
POST   /auth/login
GET    /posts
GET    /posts/:id
POST   /posts
PATCH  /posts/:id
DELETE /posts/:id
POST   /posts/:id/comments
GET    /tags
```

**Evaluation:**
```json
{
  "functionality": "40% - All endpoints work correctly",
  "code_quality": "30% - Clean, well-organized code",
  "validation": "15% - Proper Zod schemas",
  "testing": "15% - Meaningful tests"
}
```

---

#### 2. Code Review: Schema Design & Migrations (Weight: 20%)

**What's Reviewed:**
- Prisma schema structure
- Relationship definitions
- Index usage
- Migration files

**Criteria:**
- [ ] Appropriate data types
- [ ] Proper indexes on foreign keys
- [ ] Clear naming conventions
- [ ] No N+1 query potential

---

#### 3. Test Coverage: Minimum 70% (Weight: 20%)

**Requirements:**
```bash
npm run test:cov
```

Must achieve:
- Overall: 70%+
- Services: 80%+
- Controllers: 60%+

**Types of Tests:**
- Unit tests for business logic
- Integration tests for API endpoints
- Edge case coverage

---

#### 4. Practical Exam: Bug Fixes (Weight: 20%)

**Format:** 2-hour timed exercise

**Scenario:** You'll receive a codebase with 3 bugs:
1. **Bug 1 (Easy)**: Validation not working on an endpoint
2. **Bug 2 (Medium)**: Race condition in database operations
3. **Bug 3 (Hard)**: Memory leak in service

**Evaluation:**
- Bug 1 fixed: 30 points
- Bug 2 fixed: 40 points
- Bug 3 fixed: 30 points
- **Passing:** 70+ points

---

### ⏰ Time Allocation

```csv
Activity,Hours,Percentage
Self-study,18,45%
Coding practice,16,40%
Mentor sessions,4,10%
Code review,2,5%
```

### 📖 Learning Resources

#### Essential
- [Prisma Documentation](https://www.prisma.io/docs) - Complete guide
- [Zod Documentation](https://zod.dev/) - Schema validation
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/) - SQL fundamentals
- [NestJS Testing Guide](https://docs.nestjs.com/fundamentals/testing) - Testing patterns

#### Internal
#### Supplementary
- [Prisma Best Practices](https://www.prisma.io/docs/guides/performance-and-optimization)
- [Testing Best Practices](https://testingjavascript.com/)

---


---

## PHASE 2: Final Project Phase (Weeks 3-4)

**Objective:** Apply your learning through one of three project options

### Project Selection

At the end of Week 2, you'll choose (with mentor guidance) ONE of these options:

#### 🏢 Option A: Internal System Project

**Best for:** Building something new that solves a real company need

**What you'll do:**
- Propose a small internal tool (e.g., API documentation portal, team dashboard)
- Design database schema and API endpoints
- Implement core features
- Deploy to internal staging environment

**Example projects:**
- Developer onboarding checklist tracker
- API rate limit monitoring dashboard  
- Internal notification service
- Simple admin panel for data management

**Deliverables:**
- Technical design document
- Working backend with 3-5 endpoints
- Basic tests (70%+ coverage)
- Deployment documentation
- Demo presentation

---

#### 🌍 Option B: Real-World Application

**Best for:** Getting immediate production experience

**What you'll do:**
- Join an existing production codebase
- Implement assigned features (3-5 tasks)
- Follow team code review process
- Ship code to production

**Example contributions:**
- Add new API endpoints (e.g., user profile update)
- Implement data validation layer
- Add email notification feature
- Fix bugs and improve existing code
- Write missing tests

**Deliverables:**
- 3+ merged pull requests
- Code reviewed by senior developers
- Tests for all new code
- Updated documentation
- Knowledge sharing presentation

---

#### 🚀 Option C: New Application

**Best for:** Full creative control and entrepreneurial experience

**What you'll do:**
- Pitch your own app idea
- Design complete system architecture
- Build backend from scratch
- Deploy to cloud platform

**Example apps:**
- Task management API
- URL shortener service
- Simple blogging platform backend
- Weather data aggregator
- Polling/voting system

**Deliverables:**
- System architecture diagram
- Complete REST API (5-10 endpoints)
- Database schema with migrations
- Authentication implementation
- Deployed to production (Render/Railway/Heroku)
- API documentation
- Final demo

---

### Week 3: Project Setup & Core Development

**Time allocation:** 40 hours

```
Day 1-2 (16 hours): Project Kickoff & Planning
├─ Meet with mentor/stakeholders
├─ Finalize requirements
├─ Design database schema
├─ Plan API endpoints
└─ Set up project structure

Day 3-4 (16 hours): Core Implementation
├─ Implement database models (Prisma)
├─ Create main API endpoints
├─ Add validation (Zod)
└─ Write initial tests

Day 5 (8 hours): Mid-week Review
├─ Demo progress to mentor
├─ Get feedback
└─ Adjust plans if needed
```

**Milestone:** Working API with database integration

---

### Week 4: Refinement & Deployment

**Time allocation:** 40 hours

```
Day 1-2 (16 hours): Feature Completion
├─ Finish remaining endpoints
├─ Add error handling
├─ Write comprehensive tests
└─ Improve code quality

Day 3 (8 hours): Testing & Documentation
├─ Achieve 70%+ test coverage
├─ Write API documentation
├─ Add README with setup instructions
└─ Create deployment guide

Day 4 (8 hours): Deployment
├─ Set up CI/CD (if Option A/C)
├─ Deploy to staging/production
├─ Test deployed application
└─ Monitor for issues

Day 5 (8 hours): Presentation & Handover
├─ Prepare demo slides
├─ Present to team/stakeholders
├─ Gather feedback
└─ Document lessons learned
```

**Final Deliverable:** Fully working, deployed project with documentation

---

## Success Criteria

To successfully complete this 1-month program:

### Technical Requirements
- ✅ Complete all Week 1-2 learning modules
- ✅ Build practice mini-blog API (Week 2)
- ✅ Complete chosen final project with all deliverables
- ✅ Write tests (minimum 70% coverage)
- ✅ Follow code standards and best practices

### Project-Specific Criteria

**For Internal System (Option A):**
- ✅ Deploy to internal environment
- ✅ 3-5 working API endpoints
- ✅ Stakeholder approval

**For Real-World App (Option B):**
- ✅ 3+ merged pull requests
- ✅ Pass code review standards
- ✅ Code deployed to production

**For New Application (Option C):**
- ✅ Deploy to public cloud
- ✅ 5-10 working endpoints
- ✅ Complete documentation

### Soft Skills
- ✅ Clear communication in daily standups
- ✅ Proactive about asking questions
- ✅ Receptive to feedback
- ✅ Good time management
- ✅ Professional presentation

---

## Assessment & Evaluation

You'll be evaluated on:

1. **Technical Skills (40%)**
   - Code quality and best practices
   - Testing coverage and quality
   - Problem-solving approach
   - Understanding of concepts

2. **Deliverables (30%)**
   - Completion of final project
   - Quality of documentation
   - Working deployment

3. **Soft Skills (30%)**
   - Communication and collaboration
   - Time management
   - Ability to learn independently
   - Professionalism

See [Evaluation Criteria](./evaluation.md) for detailed rubrics.

---

## What Happens After?

Upon successful completion:

### Immediate Next Steps
1. **Certificate of Completion** - Official recognition from program
2. **Final Evaluation** - Written feedback and skill assessment
3. **Portfolio Piece** - Add your project to GitHub/resume
4. **LinkedIn Recommendation** - From mentor (if earned)

### Career Opportunities

**Strong performers may be offered:**
- Junior Backend Developer position
- Extended internship (2-3 month program)
- Freelance project opportunities
- Referral to partner companies

### Continued Learning

Even if not hired immediately:
- You have a production-ready portfolio project
- Clear understanding of job requirements
- Network of professional contacts
- Recommendation letter from mentor

See [Career Paths](./career-paths.md) for detailed guidance on next steps.

---

## Tips for Success

### Week 1-2 (Learning Phase)
```
DO:
├─ Take detailed notes
├─ Ask questions early and often
├─ Build practice projects
├─ Review concepts daily
└─ Connect with other interns

AVOID:
├─ Tutorial hell (passive watching)
├─ Skipping fundamentals
├─ Isolating yourself
└─ Comparing progress to others
```

### Week 3-4 (Project Phase)
```
DO:
├─ Communicate progress daily
├─ Ask for code reviews early
├─ Test as you build
├─ Document as you go
└─ Manage time wisely

AVOID:
├─ Scope creep (keep it simple)
├─ Perfectionism (ship working code)
├─ Last-minute work
└─ Skipping tests
```

---

## Frequently Asked Questions

**Q: Can I switch project options mid-way?**  
A: Generally no. Choose carefully at the end of Week 2. However, in exceptional circumstances, discuss with your mentor.

**Q: What if I fall behind in Week 1-2?**  
A: Communicate immediately with your mentor. You may need to extend learning phase or adjust project scope.

**Q: Can I work on my own project idea even if it's not "internal"?**  
A: Yes! That's Option C (New Application). Discuss your idea with mentor for approval.

**Q: What if my PRs aren't getting merged? (Option B)**  
A: This is normal. Focus on incorporating feedback and improving. Quality > quantity.

**Q: Do I need to deploy to specific platforms?**  
A: No, you can choose: Railway, Render, Heroku, AWS, GCP, Azure. Mentor will guide you.

**Q: Can I use libraries not in the core stack?**  
A: Yes, with mentor approval. Stay close to NestJS/Prisma/Zod unless there's a good reason.

See [Full FAQs](./faqs.md) for more questions.

---

## Ready to Start?

### Pre-Program Checklist
- [ ] Read [Prerequisites Guide](./prerequisites.md)
- [ ] Complete environment setup
- [ ] Install required tools (Node.js, PostgreSQL, VS Code)
- [ ] Join program Slack/Discord channel
- [ ] Meet your mentor
- [ ] Review [Tech Stack Details](./tech-stack.md)

### Day 1 Agenda
1. **Orientation Meeting (2 hours)**
   - Meet the team
   - Tour of systems and tools
   - Set up accounts and access
   
2. **Environment Setup (4 hours)**
   - Clone repositories
   - Install dependencies
   - Run project locally
   - Fix any setup issues

3. **First Learning Module (2 hours)**
   - Begin TypeScript fundamentals
   - Complete first exercises

---

## Navigation
[← Back to Overview](./overview.md) | [Next: 2-Month Program →](./2-month-program.md)

**Related:** [Skills Matrix](./skills-matrix.md) | [Evaluation Criteria](./evaluation.md) | [Success Stories](./success-stories.md)

---

**Questions?** Reach out to your mentor or check the [FAQs](./faqs.md).

**Need help?** See [Quick Reference](./appendix.md) for commands and troubleshooting.
