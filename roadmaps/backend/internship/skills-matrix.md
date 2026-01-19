# Skills Matrix

A comprehensive breakdown of expected competency levels across different program durations.

## Navigation
[← Back to Overview](./overview.md) | [Tech Stack →](./tech-stack.md)

**Related:** [1-Month Program](./1-month-program.md) | [2-Month Program](./2-month-program.md) | [3-Month Program](./3-month-program.md)

---

## Skills by Program Duration

### Quick Comparison

```csv
Skill,1 Month,2 Months,3 Months
TypeScript,Intermediate,Advanced,Expert
NestJS,Beginner,Intermediate,Advanced
Prisma ORM,Beginner,Intermediate,Advanced
PostgreSQL,Basic,Intermediate,Advanced
Zod Validation,Beginner,Intermediate,Advanced
REST API Design,Beginner,Intermediate,Expert
Testing,Basic,Intermediate,Advanced
Git & Collaboration,Intermediate,Advanced,Expert
System Design,N/A,Beginner,Intermediate
DevOps & Deployment,Basic,Intermediate,Advanced
Code Review,Receiving Only,Giving & Receiving,Leading Reviews
Documentation,Basic,Good,Excellent
Architecture Patterns,N/A,Basic,Advanced
Performance Optimization,N/A,Basic,Intermediate
Security,Basic,Intermediate,Advanced
```

---

## Proficiency Level Definitions

### 1. Basic
**What it means:** Can perform simple tasks with guidance and supervision.

**Characteristics:**
- Understands fundamental concepts
- Needs documentation/examples to work
- Requires frequent mentor input
- Can complete straightforward tasks
- Learning actively

**Example (PostgreSQL - Basic):**
```sql
-- Can write simple queries
SELECT * FROM users WHERE email = 'user@example.com';

-- Understands basic joins
SELECT posts.title, users.name 
FROM posts 
JOIN users ON posts.user_id = users.id;

-- Knows when to add indexes
CREATE INDEX idx_users_email ON users(email);
```

---

### 2. Beginner
**What it means:** Can perform common tasks independently but may struggle with edge cases.

**Characteristics:**
- Solid grasp of fundamentals
- Can work independently on defined tasks
- Recognizes patterns from examples
- Needs help with complex scenarios
- Can debug simple issues

**Example (NestJS - Beginner):**
```typescript
// Can create standard CRUD endpoints
@Controller('users')
export class UsersController {
  constructor(private usersService: UsersService) {}

  @Get()
  findAll() {
    return this.usersService.findAll();
  }

  @Post()
  create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }
}

// Understands dependency injection
@Injectable()
export class UsersService {
  constructor(private prisma: PrismaService) {}
  
  // Can implement business logic
  async findAll() {
    return this.prisma.user.findMany();
  }
}
```

---

### 3. Intermediate
**What it means:** Can handle complex scenarios and help others with basics.

**Characteristics:**
- Deep understanding of core concepts
- Can design solutions independently
- Handles edge cases confidently
- Can mentor beginners
- Proactive problem solver
- Writes clean, maintainable code

**Example (TypeScript - Intermediate):**
```typescript
// Uses advanced types effectively
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

// Implements generic utilities
class Repository<T extends { id: string }> {
  constructor(private items: T[] = []) {}

  findById(id: string): T | undefined {
    return this.items.find(item => item.id === id);
  }

  findMany(predicate: (item: T) => boolean): T[] {
    return this.items.filter(predicate);
  }
}

// Uses conditional types
type ApiResponse<T, E = Error> = 
  | { success: true; data: T }
  | { success: false; error: E };

async function fetchData<T>(url: string): Promise<ApiResponse<T>> {
  // Implementation
}
```

---

### 4. Advanced
**What it means:** Can architect solutions and mentor others effectively.

**Characteristics:**
- Expert-level knowledge
- Designs scalable architectures
- Optimizes for performance and maintainability
- Mentors intermediate developers
- Identifies and solves complex problems
- Contributes to technical decisions

**Example (NestJS - Advanced):**
```typescript
// Implements clean architecture
// Domain Layer
export class User {
  constructor(
    public readonly id: string,
    public email: string,
    private password: string,
  ) {}

  updateEmail(newEmail: string) {
    // Business rules
    if (!this.isValidEmail(newEmail)) {
      throw new Error('Invalid email');
    }
    this.email = newEmail;
  }

  private isValidEmail(email: string): boolean {
    // Validation logic
  }
}

// Application Layer
export class UpdateUserEmailUseCase {
  constructor(
    private userRepository: IUserRepository,
    private eventBus: EventBus,
  ) {}

  async execute(userId: string, newEmail: string): Promise<void> {
    const user = await this.userRepository.findById(userId);
    if (!user) throw new NotFoundException();

    user.updateEmail(newEmail);
    await this.userRepository.save(user);
    
    this.eventBus.publish(new UserEmailUpdatedEvent(userId, newEmail));
  }
}

// Infrastructure Layer
@Injectable()
export class PrismaUserRepository implements IUserRepository {
  // Implementation details
}
```

---

### 5. Expert
**What it means:** Deep mastery with ability to handle any scenario and lead technical initiatives.

**Characteristics:**
- Comprehensive understanding
- Recognized authority on the topic
- Influences technical direction
- Solves novel/unprecedented problems
- Trains advanced developers
- Drives best practices

**Example (REST API Design - Expert):**
```typescript
// Designs comprehensive API standards
/**
 * API Design Principles:
 * 1. Resource-based URLs
 * 2. HTTP methods for operations
 * 3. Consistent response structure
 * 4. Versioning strategy
 * 5. Pagination, filtering, sorting
 * 6. Error handling
 * 7. Rate limiting
 * 8. Caching headers
 * 9. HATEOAS (hypermedia)
 * 10. OpenAPI documentation
 */

// Implements advanced patterns
@Controller({ path: 'users', version: '2' })
@ApiTags('users')
export class UsersV2Controller {
  @Get()
  @ApiOperation({ summary: 'List users with advanced filtering' })
  @ApiPaginatedResponse(UserDto)
  @CacheControl('public', 300)
  async findAll(
    @Query() query: PaginationQueryDto,
    @Query() filter: UserFilterDto,
    @Query() sort: SortQueryDto,
  ): Promise<PaginatedResponse<UserDto>> {
    const result = await this.usersService.findAll({
      pagination: query,
      filter,
      sort,
    });

    return {
      data: result.items,
      meta: {
        page: query.page,
        limit: query.limit,
        total: result.total,
        totalPages: Math.ceil(result.total / query.limit),
      },
      links: {
        self: `/v2/users?page=${query.page}`,
        first: `/v2/users?page=1`,
        last: `/v2/users?page=${Math.ceil(result.total / query.limit)}`,
        next: query.page < Math.ceil(result.total / query.limit) 
          ? `/v2/users?page=${query.page + 1}` 
          : null,
        prev: query.page > 1 ? `/v2/users?page=${query.page - 1}` : null,
      },
    };
  }
}
```

---

## Detailed Skill Breakdown

### TypeScript

#### Beginner (N/A for this program)
- Basic types (string, number, boolean)
- Interfaces and simple types
- Functions with type annotations

#### Intermediate (1-Month Program)
```typescript
// Can use:
- Generic functions and classes
- Union and intersection types
- Type guards
- Utility types (Pick, Omit, Partial)
- Enums
- Type assertions

// Example
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

type UserUpdate = Partial<Pick<User, 'name' | 'email'>>;
```

#### Advanced (2-Month Program)
```typescript
// Can use:
- Advanced generics with constraints
- Conditional types
- Mapped types
- Template literal types
- Decorators
- Type inference

// Example
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends object 
    ? DeepReadonly<T[P]> 
    : T[P];
};

type EventMap = {
  'user:created': { id: string; email: string };
  'user:updated': { id: string; changes: Partial<User> };
};

type EventHandler<K extends keyof EventMap> = (
  event: EventMap[K]
) => void | Promise<void>;
```

#### Expert (3-Month Program)
```typescript
// Mastery of:
- Complex type transformations
- Recursive types
- Type-level programming
- Advanced decorator patterns
- Module augmentation

// Example
type PathsToStringProps<T> = {
  [K in keyof T]: T[K] extends string 
    ? K 
    : T[K] extends object 
    ? `${K & string}.${PathsToStringProps<T[K]> & string}` 
    : never;
}[keyof T];

type UserPaths = PathsToStringProps<{
  name: string;
  address: { street: string; city: string };
}>;
// Result: "name" | "address.street" | "address.city"
```

---

### NestJS

#### Beginner (1-Month Program)
**Can build:**
- Basic CRUD APIs
- Simple modules, controllers, services
- Use dependency injection
- Basic validation with pipes
- Simple guards for authentication

**Understands:**
- Request lifecycle
- Module system
- Providers and injection
- Basic decorators (@Get, @Post, @Body, etc.)

#### Intermediate (2-Month Program)
**Can build:**
- Complex authentication systems
- Custom guards, interceptors, pipes
- Exception filters
- Custom decorators
- Swagger documentation
- Middleware

**Understands:**
- Advanced DI patterns
- Request/response transformation
- Error handling strategies
- API design patterns

#### Advanced (3-Month Program)
**Can build:**
- Clean architecture implementations
- CQRS pattern
- Event-driven systems
- Microservices
- Advanced middleware chains
- Performance optimization

**Understands:**
- Architectural patterns
- Scalability considerations
- Testing strategies
- Production best practices

---

### Testing

#### Basic (1-Month Program)
```typescript
// Can write:
- Simple unit tests for services
- Basic integration tests for controllers
- Uses Jest matchers
- Mocks simple dependencies

describe('UserService', () => {
  it('should create user', async () => {
    const result = await service.createUser(mockData);
    expect(result).toBeDefined();
    expect(result.email).toBe(mockData.email);
  });
});
```

#### Intermediate (2-Month Program)
```typescript
// Can write:
- Comprehensive unit tests
- Integration tests with database
- E2E tests for critical flows
- Complex mocking strategies
- Achieves 80%+ coverage

describe('UserService', () => {
  let service: UserService;
  let prisma: DeepMockProxy<PrismaService>;

  beforeEach(() => {
    prisma = mockDeep<PrismaService>();
    service = new UserService(prisma);
  });

  describe('createUser', () => {
    it('should create user with hashed password', async () => {
      prisma.user.create.mockResolvedValue(mockUser);
      
      const result = await service.createUser(createDto);
      
      expect(prisma.user.create).toHaveBeenCalledWith({
        data: expect.objectContaining({
          email: createDto.email,
          password: expect.not.stringMatching(createDto.password),
        })
      });
    });

    it('should throw if email exists', async () => {
      prisma.user.findUnique.mockResolvedValue(existingUser);
      
      await expect(service.createUser(createDto))
        .rejects
        .toThrow(ConflictException);
    });
  });
});
```

#### Advanced (3-Month Program)
```typescript
// Can write:
- TDD (test-first development)
- Comprehensive test suites (unit + integration + e2e)
- Performance tests
- Security tests
- Achieves 85%+ meaningful coverage

// Practices TDD
describe('AssignTaskUseCase', () => {
  // Write test first
  it('should assign task and emit event', async () => {
    const command = new AssignTaskCommand('task-1', 'user-1');
    
    await useCase.execute(command);
    
    expect(repository.save).toHaveBeenCalledWith(
      expect.objectContaining({ assigneeId: 'user-1' })
    );
    expect(eventBus.publish).toHaveBeenCalledWith(
      expect.any(TaskAssignedEvent)
    );
  });

  // Then implement the use case
});

// Advanced mocking
const mockEventBus = {
  publish: jest.fn(),
};

const mockRepository = {
  findById: jest.fn().mockImplementation((id) => {
    return id === 'task-1' ? mockTask : null;
  }),
  save: jest.fn(),
};
```

---

### System Design

#### N/A (1-Month Program)
Not covered in depth. Focus is on implementation.

#### Beginner (2-Month Program)
**Can design:**
- Simple system architectures
- Database schemas for features
- API contracts
- Basic component diagrams

**Understands:**
- Separation of concerns
- Component relationships
- Data flow
- Basic scalability concepts

#### Intermediate (3-Month Program)
**Can design:**
- Complex system architectures
- Scalable database schemas
- Microservices boundaries
- Caching strategies
- Queue-based systems

**Understands:**
- CAP theorem
- Eventual consistency
- Load balancing
- Horizontal vs vertical scaling
- Monitoring strategies

---

### Git & Collaboration

#### Intermediate (1-Month Program)
```bash
# Can do:
- Create feature branches
- Write clear commit messages
- Submit pull requests
- Respond to code review feedback
- Resolve simple merge conflicts

# Example workflow
git checkout -b feature/add-user-endpoint
# Make changes
git add .
git commit -m "feat: add user registration endpoint

- Add POST /users endpoint
- Implement email validation
- Add unit tests
"
git push origin feature/add-user-endpoint
# Create PR, respond to feedback
```

#### Advanced (2-Month Program)
```bash
# Can do:
- All of Intermediate
- Give meaningful code reviews
- Manage release branches
- Use git rebase effectively
- Interactive rebase for clean history

# Example review comment
"""
Consider extracting the validation logic into a separate validator class
for better reusability and testability:

class EmailValidator {
  static validate(email: string): boolean {
    // Validation logic
  }
}

This follows Single Responsibility Principle and makes unit testing easier.
"""
```

#### Expert (3-Month Program)
```bash
# Can do:
- All of Advanced
- Lead code review process
- Define git workflows for teams
- Resolve complex merge conflicts
- Mentor on git best practices

# Advanced operations
git rebase -i HEAD~5  # Interactive rebase
git bisect start      # Binary search for bugs
git cherry-pick abc123 # Selective commit application
```

---

## Skills Growth Timeline

### Month 1
```json
{
  "week_1": {
    "TypeScript": "Beginner → Intermediate",
    "NestJS": "None → Beginner",
    "Git": "Basic → Intermediate"
  },
  "week_2": {
    "Prisma": "None → Beginner",
    "PostgreSQL": "None → Basic",
    "Testing": "None → Basic",
    "Zod": "None → Beginner"
  },
  "week_3_4": {
    "Collaboration": "Observer → Participant",
    "Code Review": "None → Receiving",
    "Documentation": "None → Basic"
  }
}
```

### Month 2
```json
{
  "week_5_6": {
    "TypeScript": "Intermediate → Advanced",
    "NestJS": "Beginner → Intermediate",
    "Authentication": "None → Intermediate",
    "API Design": "Beginner → Intermediate"
  },
  "week_7_8": {
    "System Design": "None → Beginner",
    "Architecture": "None → Basic",
    "DevOps": "Basic → Intermediate",
    "Testing": "Basic → Intermediate"
  }
}
```

### Month 3
```json
{
  "week_9_12": {
    "NestJS": "Intermediate → Advanced",
    "Architecture": "Basic → Advanced",
    "System Design": "Beginner → Intermediate",
    "Testing": "Intermediate → Advanced",
    "Performance": "None → Intermediate",
    "Security": "Basic → Advanced"
  }
}
```

---

## Self-Assessment Checklist

Use this to evaluate your current level and identify growth areas.

### TypeScript
- [ ] I can define interfaces and types
- [ ] I use generics comfortably
- [ ] I understand utility types (Pick, Omit, etc.)
- [ ] I can create custom utility types
- [ ] I use conditional types
- [ ] I implement decorators

### NestJS
- [ ] I can create modules, controllers, services
- [ ] I understand dependency injection
- [ ] I can implement guards
- [ ] I can create interceptors
- [ ] I can build custom decorators
- [ ] I understand clean architecture

### Testing
- [ ] I write unit tests for services
- [ ] I write integration tests for APIs
- [ ] I can mock dependencies effectively
- [ ] I practice TDD
- [ ] I maintain 80%+ test coverage
- [ ] I write meaningful tests (not just coverage)

### Database (Prisma/PostgreSQL)
- [ ] I can design database schemas
- [ ] I understand relationships (1-to-1, 1-to-many, many-to-many)
- [ ] I know when to add indexes
- [ ] I can write complex queries
- [ ] I understand transactions
- [ ] I can optimize query performance

### API Design
- [ ] I follow RESTful conventions
- [ ] I implement pagination
- [ ] I handle errors consistently
- [ ] I document APIs with Swagger
- [ ] I implement versioning
- [ ] I consider performance and caching

### DevOps & Deployment
- [ ] I can set up CI/CD pipelines
- [ ] I can containerize applications (Docker)
- [ ] I understand environment configuration
- [ ] I can deploy to cloud platforms
- [ ] I set up monitoring and logging
- [ ] I understand deployment strategies

---

## What Employers Look For

### Junior Backend Engineer
**Required Skills (from our program):**
- ✅ TypeScript: Intermediate
- ✅ NestJS: Beginner-Intermediate
- ✅ Database: Basic-Intermediate
- ✅ Testing: Basic-Intermediate
- ✅ Git: Intermediate
- ✅ API Design: Beginner

**Achieved by:** 1-2 Month Program

---

### Mid-Level Backend Engineer
**Required Skills:**
- ✅ TypeScript: Advanced
- ✅ NestJS: Intermediate-Advanced
- ✅ Database: Intermediate-Advanced
- ✅ Testing: Intermediate
- ✅ System Design: Beginner-Intermediate
- ✅ DevOps: Intermediate

**Achieved by:** 2-3 Month Program

---

### Senior Backend Engineer
**Required Skills (beyond this program):**
- Expert TypeScript
- Expert NestJS
- Advanced System Design
- Advanced Architecture
- Team Leadership
- Mentoring

**Timeline:** 2-3 years after completing 3-month program

---

## Next Steps

Based on your current level, choose your path:

- **Complete Beginner?** → Start with [1-Month Program](./1-month-program.md)
- **Some Experience?** → Try [2-Month Program](./2-month-program.md)
- **Want Deep Mastery?** → Go for [3-Month Program](./3-month-program.md)

Review [Prerequisites](./prerequisites.md) before starting.

---

[← Back to Overview](./overview.md) | [Evaluation Criteria →](./evaluation.md)
