---
id: 2-month-program
---

# 2-Month Internship Program

**Objective**: Master advanced backend concepts and build a substantial project with full ownership.

## Navigation
[← Back to Overview](./overview.md) | [← 1-Month Program](./1-month-program.md) | [Next: 3-Month Program →](./3-month-program.md)

**Related:** [Tech Stack](./tech-stack.md) | [Skills Matrix](./skills-matrix.md) | [Evaluation](./evaluation.md)

---

## Program Structure

This 2-month program follows a **two-phase approach**:

```
┌──────────────────────────────────────────────────────────┐
│              2-MONTH INTERNSHIP PROGRAM                  │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  PHASE 1: LEARNING (Weeks 1-4)                          │
│  ├─ Weeks 1-2: Advanced TypeScript + NestJS             │
│  ├─ Weeks 3-4: System Design + Project Planning         │
│  └─ Build: Authentication system + Project proposal     │
│                                                          │
│  PHASE 2: FINAL PROJECT (Weeks 5-8)                     │
│  Choose ONE option:                                      │
│  ├─ 🏢 Internal System (Mid-size internal tool)         │
│  ├─ 🌍 Real-World App (Major feature contribution)      │
│  └─ 🚀 New Application (Complete app with deployment)   │
│                                                          │
│  ├─ Weeks 5-6: Core development + testing               │
│  └─ Weeks 7-8: Deployment + presentation                │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### What You'll Achieve

By the end of this program:
- ✅ Master advanced NestJS patterns (guards, interceptors, custom decorators)
- ✅ Design systems from scratch with proper architecture
- ✅ Build production-grade authentication and authorization
- ✅ Own a complete feature end-to-end (design → deployment)
- ✅ Deploy to production with CI/CD and monitoring
- ✅ Be **ready for mid-level developer roles**

---

## PHASE 1: Learning Phase (Weeks 1-4)

**Objective:** Master advanced backend concepts and prepare for substantial project work

### Weeks 1-2: Advanced Fundamentals

### 🎯 Learning Objectives / Competencies

By the end of Phase 1, you will be able to:
- **All competencies from 1-month program** (if you haven't done it)
- Use advanced TypeScript patterns (decorators, conditional types)
- Implement authentication and authorization systems
- Create custom NestJS guards, interceptors, and pipes
- Design RESTful APIs following industry best practices
- Document APIs with Swagger/OpenAPI

### 📚 Core Materials

```csv
Week,Topic,Subtopics,Priority,Hours
1,Advanced TypeScript,"Decorators Advanced generics Conditional types Mapped types",High,8
1,NestJS Advanced,"Guards Interceptors Custom decorators Exception filters Middleware",High,10
1,Design Patterns,"Dependency Injection Factory Strategy Repository",Medium,4
2,Authentication,"JWT Strategy Refresh tokens Role-based access Password hashing",High,8
2,Authorization,"Guards Decorators RBAC ABAC",High,4
2,API Design,"REST best practices Swagger/OpenAPI Versioning Pagination",Medium,6
2,Security,"OWASP Top 10 Input validation Rate limiting CORS",High,4
```

### 🔨 Learning Activities

#### Week 1: Advanced TypeScript & NestJS Patterns (40 hours)

**Day 1-2: Advanced TypeScript (16 hours)**

**Goals:**
- Master decorators
- Understand advanced type patterns
- Use utility types effectively

**Topics:**

1. **Decorators (4 hours)**
   ```typescript
   // Method decorator
   function Log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
     const originalMethod = descriptor.value;
     
     descriptor.value = async function (...args: any[]) {
       console.log(`Calling ${propertyKey} with args:`, args);
       const result = await originalMethod.apply(this, args);
       console.log(`Result:`, result);
       return result;
     };
     
     return descriptor;
   }

   // Usage
   class UserService {
     @Log
     async createUser(data: CreateUserDto) {
       // ...
     }
   }

   // Parameter decorator
   function ValidateEmail(target: any, propertyKey: string, parameterIndex: number) {
     // Store metadata for validation
   }

   // Class decorator
   function ApiController(prefix: string) {
     return function (constructor: Function) {
       // Add metadata to class
       Reflect.defineMetadata('prefix', prefix, constructor);
     };
   }
   ```

2. **Advanced Generics (4 hours)**
   ```typescript
   // Generic constraints
   interface Identifiable {
     id: number;
   }

   function findById<T extends Identifiable>(items: T[], id: number): T | undefined {
     return items.find(item => item.id === id);
   }

   // Generic utility types
   type PartialBy<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>;

   interface User {
     id: number;
     name: string;
     email: string;
   }

   // Make 'email' optional
   type UpdateUserDto = PartialBy<User, 'email'>;

   // Conditional types
   type IsString<T> = T extends string ? true : false;
   type A = IsString<string>; // true
   type B = IsString<number>; // false

   // Mapped types
   type Nullable<T> = {
     [K in keyof T]: T[K] | null;
   };

   type NullableUser = Nullable<User>;
   // { id: number | null; name: string | null; email: string | null }
   ```

3. **Practical Project (8 hours)**
   - Build: **Type-safe API Client Generator**
   - Use decorators to define API endpoints
   - Generate TypeScript types from schemas
   - Implement generic CRUD operations

**Day 3-5: Advanced NestJS Patterns (24 hours)**

**Goals:**
- Master guards, interceptors, pipes
- Create reusable decorators
- Implement middleware

**Topics:**

1. **Guards - Authorization (6 hours)**
   ```typescript
   // Auth guard
   @Injectable()
   export class JwtAuthGuard implements CanActivate {
     constructor(private jwtService: JwtService) {}

     async canActivate(context: ExecutionContext): Promise<boolean> {
       const request = context.switchToHttp().getRequest();
       const token = this.extractToken(request);
       
       if (!token) {
         throw new UnauthorizedException('No token provided');
       }

       try {
         const payload = await this.jwtService.verifyAsync(token);
         request.user = payload;
         return true;
       } catch {
         throw new UnauthorizedException('Invalid token');
       }
     }

     private extractToken(request: Request): string | undefined {
       const [type, token] = request.headers.authorization?.split(' ') ?? [];
       return type === 'Bearer' ? token : undefined;
     }
   }

   // Roles guard
   @Injectable()
   export class RolesGuard implements CanActivate {
     constructor(private reflector: Reflector) {}

     canActivate(context: ExecutionContext): boolean {
       const requiredRoles = this.reflector.get<string[]>('roles', context.getHandler());
       if (!requiredRoles) return true;

       const request = context.switchToHttp().getRequest();
       const user = request.user;

       return requiredRoles.some(role => user.roles?.includes(role));
     }
   }

   // Usage
   @Controller('admin')
   @UseGuards(JwtAuthGuard, RolesGuard)
   export class AdminController {
     @Get('users')
     @Roles('admin')
     getUsers() {
       // Only admins can access
     }
   }
   ```

2. **Interceptors - Cross-cutting Concerns (6 hours)**
   ```typescript
   // Logging interceptor
   @Injectable()
   export class LoggingInterceptor implements NestInterceptor {
     intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
       const request = context.switchToHttp().getRequest();
       const { method, url } = request;
       const now = Date.now();

       return next.handle().pipe(
         tap(() => {
           const response = context.switchToHttp().getResponse();
           const delay = Date.now() - now;
           console.log(`${method} ${url} ${response.statusCode} - ${delay}ms`);
         })
       );
     }
   }

   // Transform response interceptor
   @Injectable()
   export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
     intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
       return next.handle().pipe(
         map(data => ({
           success: true,
           data,
           timestamp: new Date().toISOString()
         }))
       );
     }
   }

   // Timeout interceptor
   @Injectable()
   export class TimeoutInterceptor implements NestInterceptor {
     constructor(private readonly timeout: number = 5000) {}

     intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
       return next.handle().pipe(
         timeout(this.timeout),
         catchError(err => {
           if (err instanceof TimeoutError) {
             throw new RequestTimeoutException('Request took too long');
           }
           throw err;
         })
       );
     }
   }
   ```

3. **Custom Decorators (4 hours)**
   ```typescript
   // User decorator
   export const CurrentUser = createParamDecorator(
     (data: keyof User | undefined, ctx: ExecutionContext) => {
       const request = ctx.switchToHttp().getRequest();
       const user = request.user;
       return data ? user?.[data] : user;
     }
   );

   // Usage
   @Get('profile')
   getProfile(@CurrentUser() user: User) {
     return user;
   }

   @Get('email')
   getEmail(@CurrentUser('email') email: string) {
     return { email };
   }

   // Roles decorator
   export const Roles = (...roles: string[]) => SetMetadata('roles', roles);

   // Public route decorator
   export const Public = () => SetMetadata('isPublic', true);

   // API documentation decorators
   export function ApiPaginatedResponse<T>(model: Type<T>) {
     return applyDecorators(
       ApiOkResponse({
         schema: {
           properties: {
             data: {
               type: 'array',
               items: { $ref: getSchemaPath(model) }
             },
             meta: {
               type: 'object',
               properties: {
                 page: { type: 'number' },
                 limit: { type: 'number' },
                 total: { type: 'number' }
               }
             }
           }
         }
       })
     );
   }
   ```

4. **Exception Filters (4 hours)**
   ```typescript
   @Catch(HttpException)
   export class HttpExceptionFilter implements ExceptionFilter {
     catch(exception: HttpException, host: ArgumentsHost) {
       const ctx = host.switchToHttp();
       const response = ctx.getResponse<Response>();
       const request = ctx.getRequest<Request>();
       const status = exception.getStatus();
       const exceptionResponse = exception.getResponse();

       response.status(status).json({
         success: false,
         statusCode: status,
         timestamp: new Date().toISOString(),
         path: request.url,
         message: typeof exceptionResponse === 'string' 
           ? exceptionResponse 
           : (exceptionResponse as any).message,
         errors: typeof exceptionResponse === 'object' 
           ? (exceptionResponse as any).errors 
           : undefined
       });
     }
   }

   // Database exception filter
   @Catch(PrismaClientKnownRequestError)
   export class PrismaExceptionFilter implements ExceptionFilter {
     catch(exception: PrismaClientKnownRequestError, host: ArgumentsHost) {
       const ctx = host.switchToHttp();
       const response = ctx.getResponse<Response>();

       switch (exception.code) {
         case 'P2002':
           response.status(409).json({
             success: false,
             message: 'Unique constraint violation',
             field: exception.meta?.target
           });
           break;
         case 'P2025':
           response.status(404).json({
             success: false,
             message: 'Record not found'
           });
           break;
         default:
           response.status(500).json({
             success: false,
             message: 'Database error'
           });
       }
     }
   }
   ```

5. **Middleware (4 hours)**
   ```typescript
   @Injectable()
   export class LoggerMiddleware implements NestMiddleware {
     use(req: Request, res: Response, next: NextFunction) {
       const { method, originalUrl } = req;
       const startTime = Date.now();

       res.on('finish', () => {
         const { statusCode } = res;
         const duration = Date.now() - startTime;
         console.log(`[${method}] ${originalUrl} - ${statusCode} (${duration}ms)`);
       });

       next();
     }
   }

   // Apply middleware
   export class AppModule implements NestModule {
     configure(consumer: MiddlewareConsumer) {
       consumer
         .apply(LoggerMiddleware)
         .forRoutes('*');
     }
   }
   ```

**Deliverable:** Project using all advanced patterns (guards, interceptors, decorators, filters, middleware)

---

#### Week 2: Authentication, Authorization & API Design (40 hours)

**Day 1-3: Complete Authentication System (24 hours)**

**Goals:**
- Implement JWT authentication
- Add refresh token mechanism
- Implement role-based access control

**Project: Full Authentication System**

1. **Setup (2 hours)**
   ```bash
   npm install @nestjs/jwt @nestjs/passport passport passport-jwt bcrypt
   npm install -D @types/passport-jwt @types/bcrypt
   ```

2. **Database Schema (2 hours)**
   ```prisma
   model User {
     id           Int       @id @default(autoincrement())
     email        String    @unique
     password     String    // Hashed
     name         String
     role         Role      @default(USER)
     refreshToken String?   // Current refresh token
     createdAt    DateTime  @default(now())
     updatedAt    DateTime  @updatedAt
   }

   enum Role {
     USER
     ADMIN
     MODERATOR
   }
   ```

3. **Auth Module Implementation (12 hours)**
   ```typescript
   // auth.service.ts
   @Injectable()
   export class AuthService {
     constructor(
       private prisma: PrismaService,
       private jwtService: JwtService,
     ) {}

     async register(dto: RegisterDto) {
       // Validate
       const exists = await this.prisma.user.findUnique({
         where: { email: dto.email }
       });
       if (exists) throw new ConflictException('Email already exists');

       // Hash password
       const hashedPassword = await bcrypt.hash(dto.password, 10);

       // Create user
       const user = await this.prisma.user.create({
         data: {
           email: dto.email,
           name: dto.name,
           password: hashedPassword
         }
       });

       // Generate tokens
       const tokens = await this.generateTokens(user.id, user.email, user.role);
       await this.updateRefreshToken(user.id, tokens.refreshToken);

       return { user: this.sanitizeUser(user), ...tokens };
     }

     async login(dto: LoginDto) {
       // Find user
       const user = await this.prisma.user.findUnique({
         where: { email: dto.email }
       });
       if (!user) throw new UnauthorizedException('Invalid credentials');

       // Verify password
       const valid = await bcrypt.compare(dto.password, user.password);
       if (!valid) throw new UnauthorizedException('Invalid credentials');

       // Generate tokens
       const tokens = await this.generateTokens(user.id, user.email, user.role);
       await this.updateRefreshToken(user.id, tokens.refreshToken);

       return { user: this.sanitizeUser(user), ...tokens };
     }

     async refreshTokens(userId: number, refreshToken: string) {
       // Get user with refresh token
       const user = await this.prisma.user.findUnique({
         where: { id: userId }
       });
       if (!user || !user.refreshToken) {
         throw new ForbiddenException('Access denied');
       }

       // Verify refresh token matches
       const valid = await bcrypt.compare(refreshToken, user.refreshToken);
       if (!valid) throw new ForbiddenException('Access denied');

       // Generate new tokens
       const tokens = await this.generateTokens(user.id, user.email, user.role);
       await this.updateRefreshToken(user.id, tokens.refreshToken);

       return tokens;
     }

     async logout(userId: number) {
       await this.prisma.user.update({
         where: { id: userId },
         data: { refreshToken: null }
       });
     }

     private async generateTokens(userId: number, email: string, role: Role) {
       const payload = { sub: userId, email, role };

       const [accessToken, refreshToken] = await Promise.all([
         this.jwtService.signAsync(payload, {
           secret: process.env.JWT_SECRET,
           expiresIn: '15m'
         }),
         this.jwtService.signAsync(payload, {
           secret: process.env.JWT_REFRESH_SECRET,
           expiresIn: '7d'
         })
       ]);

       return { accessToken, refreshToken };
     }

     private async updateRefreshToken(userId: number, refreshToken: string) {
       const hashed = await bcrypt.hash(refreshToken, 10);
       await this.prisma.user.update({
         where: { id: userId },
         data: { refreshToken: hashed }
       });
     }

     private sanitizeUser(user: User) {
       const { password, refreshToken, ...sanitized } = user;
       return sanitized;
     }
   }

   // auth.controller.ts
   @Controller('auth')
   export class AuthController {
     constructor(private authService: AuthService) {}

     @Post('register')
     register(@Body(new ZodValidationPipe(RegisterSchema)) dto: RegisterDto) {
       return this.authService.register(dto);
     }

     @Post('login')
     login(@Body(new ZodValidationPipe(LoginSchema)) dto: LoginDto) {
       return this.authService.login(dto);
     }

     @Post('refresh')
     @UseGuards(RefreshTokenGuard)
     refresh(@CurrentUser() user: User, @Body('refreshToken') token: string) {
       return this.authService.refreshTokens(user.id, token);
     }

     @Post('logout')
     @UseGuards(JwtAuthGuard)
     logout(@CurrentUser('id') userId: number) {
       return this.authService.logout(userId);
     }

     @Get('me')
     @UseGuards(JwtAuthGuard)
     getProfile(@CurrentUser() user: User) {
       return user;
     }
   }
   ```

4. **Testing (8 hours)**
   - Unit tests for auth service
   - E2E tests for auth flow
   - Test token expiration
   - Test refresh token rotation
   - Test unauthorized access

**Day 4-5: API Design & Documentation (16 hours)**

**Goals:**
- Design RESTful APIs
- Document with Swagger
- Implement pagination, filtering, sorting

**Topics:**

1. **REST API Best Practices (4 hours)**
   ```typescript
   // Consistent response structure
   interface ApiResponse<T> {
     success: boolean;
     data?: T;
     meta?: PaginationMeta;
     error?: {
       message: string;
       code: string;
       details?: any;
     };
   }

   // Pagination
   interface PaginationMeta {
     page: number;
     limit: number;
     total: number;
     totalPages: number;
     hasNext: boolean;
     hasPrev: boolean;
   }

   // Filtering & sorting
   const PostQuerySchema = z.object({
     page: z.coerce.number().int().positive().default(1),
     limit: z.coerce.number().int().positive().max(100).default(20),
     sort: z.enum(['createdAt', 'updatedAt', 'title']).default('createdAt'),
     order: z.enum(['asc', 'desc']).default('desc'),
     search: z.string().optional(),
     published: z.coerce.boolean().optional(),
     authorId: z.coerce.number().int().optional(),
   });

   @Get()
   async findAll(@Query() query: PostQueryDto) {
     const { page, limit, sort, order, search, published, authorId } = 
       PostQuerySchema.parse(query);

     const where: Prisma.PostWhereInput = {
       ...(search && {
         OR: [
           { title: { contains: search, mode: 'insensitive' } },
           { content: { contains: search, mode: 'insensitive' } }
         ]
       }),
       ...(published !== undefined && { published }),
       ...(authorId && { authorId })
     };

     const [posts, total] = await Promise.all([
       this.prisma.post.findMany({
         where,
         skip: (page - 1) * limit,
         take: limit,
         orderBy: { [sort]: order },
         include: { author: { select: { id: true, name: true } } }
       }),
       this.prisma.post.count({ where })
     ]);

     const totalPages = Math.ceil(total / limit);

     return {
       success: true,
       data: posts,
       meta: {
         page,
         limit,
         total,
         totalPages,
         hasNext: page < totalPages,
         hasPrev: page > 1
       }
     };
   }
   ```

2. **Swagger Documentation (6 hours)**
   ```typescript
   // main.ts
   const config = new DocumentBuilder()
     .setTitle('Blog API')
     .setDescription('Complete blog API with authentication')
     .setVersion('1.0')
     .addBearerAuth()
     .addTag('auth', 'Authentication endpoints')
     .addTag('posts', 'Blog posts management')
     .build();

   const document = SwaggerModule.createDocument(app, config);
   SwaggerModule.setup('api', app, document);

   // posts.controller.ts
   @ApiTags('posts')
   @Controller('posts')
   export class PostsController {
     @Get()
     @ApiOperation({ summary: 'Get all posts with pagination' })
     @ApiQuery({ name: 'page', required: false, type: Number })
     @ApiQuery({ name: 'limit', required: false, type: Number })
     @ApiQuery({ name: 'search', required: false, type: String })
     @ApiResponse({ 
       status: 200, 
       description: 'Posts retrieved successfully',
       type: [Post]
     })
     findAll(@Query() query: PostQueryDto) {
       // ...
     }

     @Post()
     @UseGuards(JwtAuthGuard)
     @ApiBearerAuth()
     @ApiOperation({ summary: 'Create new post' })
     @ApiBody({ type: CreatePostDto })
     @ApiResponse({ status: 201, description: 'Post created', type: Post })
     @ApiResponse({ status: 401, description: 'Unauthorized' })
     create(@Body() dto: CreatePostDto, @CurrentUser() user: User) {
       // ...
     }
   }
   ```

3. **API Versioning (2 hours)**
   ```typescript
   // Enable versioning
   app.enableVersioning({
     type: VersioningType.URI,
   });

   // v1 controller
   @Controller({ path: 'posts', version: '1' })
   export class PostsV1Controller {
     // Old implementation
   }

   // v2 controller
   @Controller({ path: 'posts', version: '2' })
   export class PostsV2Controller {
     // New implementation with breaking changes
   }
   ```

4. **Rate Limiting (2 hours)**
   ```typescript
   npm install @nestjs/throttler

   // app.module.ts
   @Module({
     imports: [
       ThrottlerModule.forRoot({
         ttl: 60,
         limit: 10,
       }),
     ],
   })

   // Apply to routes
   @UseGuards(ThrottlerGuard)
   @Controller('auth')
   export class AuthController {
     // 10 requests per minute
   }
   ```

5. **CORS & Security (2 hours)**
   ```typescript
   // main.ts
   app.enableCors({
     origin: process.env.ALLOWED_ORIGINS?.split(','),
     credentials: true,
   });

   app.use(helmet());
   ```

**Deliverable:** Complete authenticated API with Swagger documentation

---

### ✅ Phase 1 Assessment

#### 1. Build: Authentication System with JWT (Weight: 40%)

**Requirements:**
- [ ] User registration with validation
- [ ] Login with JWT access tokens
- [ ] Refresh token mechanism
- [ ] Role-based access control (USER, ADMIN)
- [ ] Password hashing with bcrypt
- [ ] Logout functionality
- [ ] Protected routes
- [ ] Swagger documentation

**Evaluation Criteria:**
```json
{
  "functionality": "50% - All features work correctly",
  "security": "30% - Proper password hashing, token validation",
  "code_quality": "10% - Clean, organized code",
  "documentation": "10% - Complete Swagger docs"
}
```

**Passing Score:** 85%

---

#### 2. Code Review: Advanced Patterns (Weight: 30%)

**What's Reviewed:**
- Guards implementation
- Interceptors usage
- Custom decorators
- Exception filters
- Proper separation of concerns

**Must Demonstrate:**
- [ ] At least 2 custom guards
- [ ] At least 2 interceptors
- [ ] At least 3 custom decorators
- [ ] Global exception filter
- [ ] Proper error handling

---

#### 3. Quiz: Security & Authentication (Weight: 20%)

**Topics:**
- JWT structure and claims
- Token expiration strategies
- Password hashing algorithms
- OWASP Top 10
- CORS and CSRF
- Rate limiting strategies

**Format:** 15 questions, 30 minutes
**Passing Score:** 85%

---

#### 4. Practical: Design API for Business Requirement (Weight: 10%)

**Given:** Business requirement document
**Task:** Design complete API specification

**Must Include:**
- Endpoint list with HTTP methods
- Request/response schemas
- Authentication requirements
- Pagination strategy
- Error handling approach

**Time:** 2 hours
**Evaluation:** Pass/Fail

---

### ⏰ Time Allocation

```json
{
  "week_1": {
    "self_study": 20,
    "coding_practice": 18,
    "mentor_sessions": 4,
    "code_review": 3
  },
  "week_2": {
    "self_study": 18,
    "coding_practice": 20,
    "mentor_sessions": 4,
    "project_planning": 3
  }
}
```

### 📖 Learning Resources

#### Essential
- [NestJS Advanced Concepts](https://docs.nestjs.com/fundamentals/custom-decorators) - Guards, interceptors, etc.
- [JWT Best Practices](https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp/) - Token security
- [REST API Design Guide](https://restfulapi.net/) - API conventions
- [Swagger/OpenAPI Documentation](https://swagger.io/docs/) - API documentation

#### Internal
- [Tech Stack Details](./tech-stack.md) - Deep dive into technologies

#### Supplementary
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) - Security vulnerabilities
- [API Security Checklist](https://github.com/shieldfy/API-Security-Checklist)

---

### Weeks 3-4: System Design & Project Planning

### 🎯 Learning Objectives / Competencies

By the end of Phase 2, you will be able to:
- Identify real business problems worth solving
- Conduct stakeholder interviews
- Write technical specifications
- Design system architecture
- Create database schemas for complex systems
- Estimate tasks and plan sprints
- Present proposals to technical teams

### 📚 Core Materials

**Topics:**
- Product requirements gathering
- Technical specification writing
- System architecture design
- Database schema design for scale
- API contract design
- Project estimation techniques

### 🔨 Learning Activities

#### Week 3: Ideation & Research (40 hours)

**Day 1-2: Problem Discovery (16 hours)**

**Goals:**
- Find real problems to solve
- Understand user needs
- Validate problem is worth solving

**Activities:**

1. **Shadow Product Team (4 hours)**
   - Attend product meetings
   - Observe feature discussions
   - Understand prioritization
   - Learn how requirements are gathered

2. **Stakeholder Interviews (6 hours)**
   - Interview 5+ internal stakeholders (engineers, PMs, designers)
   - Questions to ask:
     - "What takes too much time in your workflow?"
     - "What manual process could be automated?"
     - "What data do you wish you had?"
     - "What frustrates you about current tools?"
   - Document pain points

3. **Research Existing Solutions (4 hours)**
   - What tools exist externally?
   - What internal tools are similar?
   - Why aren't current solutions sufficient?
   - What can we learn from them?

4. **Brainstorm Project Ideas (2 hours)**
   - Generate 5-10 ideas with mentor
   - Evaluate each:
     - **Impact**: How much value does it provide?
     - **Feasibility**: Can it be done in 4 weeks?
     - **Learning**: Does it teach new skills?
     - **Interest**: Are you excited about it?

**Example Project Ideas:**
```csv
Idea,Impact,Feasibility,Learning,Score
Invoice Generation System,High,Medium,High,8/10
Internal Admin Dashboard,Medium,High,Medium,7/10
Audit Log System,High,High,Medium,8/10
Notification Service,Medium,High,High,8/10
API Rate Analytics,Medium,Medium,High,7/10
```

**Day 3-5: Project Selection & Scope (24 hours)**

**Goals:**
- Select one project
- Define clear scope
- Write project proposal

**Activities:**

1. **Validate Choice with Mentor (2 hours)**
   - Present top 3 ideas
   - Get feedback on feasibility
   - Select final project
   - Agree on scope

2. **Define Project Scope (6 hours)**
   - **Must Have** (Core features for MVP)
   - **Should Have** (Important but not critical)
   - **Could Have** (Nice to have if time permits)
   - **Won't Have** (Out of scope)

   **Example: Invoice Generation System**
   ```json
   {
     "must_have": [
       "Generate PDF invoices from templates",
       "Store invoice data in database",
       "Email invoices to customers",
       "View invoice history"
     ],
     "should_have": [
       "Multiple invoice templates",
       "Invoice status tracking (draft, sent, paid)",
       "Basic reporting"
     ],
     "could_have": [
       "Payment integration",
       "Recurring invoices",
       "Multi-currency support"
     ],
     "wont_have": [
       "Mobile app",
       "Advanced analytics",
       "Integration with accounting software"
     ]
   }
   ```

3. **Write Project Proposal (12 hours)**

   **Proposal Structure:**
   ```markdown
   # Project Proposal: [Project Name]

   ## 1. Problem Statement
   - What problem are we solving?
   - Who experiences this problem?
   - How are they solving it now?
   - Why is the current solution inadequate?

   ## 2. Proposed Solution
   - High-level description
   - Key features
   - Success criteria
   - Non-goals

   ## 3. User Stories
   As a [role], I want to [action] so that [benefit]

   ## 4. Technical Approach
   - Architecture overview
   - Tech stack justification
   - Key technical decisions

   ## 5. Scope
   - Must have (MVP)
   - Should have
   - Could have
   - Won't have

   ## 6. Success Metrics
   - How will we measure success?
   - What KPIs matter?

   ## 7. Risks & Mitigations
   - What could go wrong?
   - How will we handle it?

   ## 8. Timeline
   - High-level milestones
   - Week-by-week plan
   ```

4. **Review & Iterate (4 hours)**
   - Present to mentor
   - Get feedback
   - Revise proposal
   - Get approval

**Deliverable:** Approved project proposal (5-10 pages)

---

#### Week 4: Technical Design (40 hours)

**Day 1-2: Architecture Design (16 hours)**

**Goals:**
- Design system architecture
- Define component boundaries
- Plan data flow

**Activities:**

1. **System Architecture (8 hours)**
   ```
   Create diagrams:
   - High-level architecture
   - Request flow diagrams
   - Database entity relationships
   - External service integrations
   ```

   **Example: Invoice System Architecture**
   ```
   ┌─────────────┐
   │   Client    │
   └──────┬──────┘
          │ HTTPS
   ┌──────▼──────────────────────┐
   │   API Gateway (NestJS)      │
   │  - Authentication           │
   │  - Rate Limiting            │
   │  - Request Validation       │
   └──────┬──────────────────────┘
          │
   ┌──────▼──────────────────────┐
   │   Business Logic Layer      │
   │  - InvoiceService           │
   │  - CustomerService          │
   │  - EmailService             │
   └──────┬──────────────────────┘
          │
   ┌──────▼─────┬────────┬───────┐
   │ PostgreSQL │ Redis  │ S3    │
   │ (Primary)  │(Cache) │(PDFs) │
   └────────────┴────────┴───────┘
          │
   ┌──────▼──────────────────────┐
   │   External Services         │
   │  - SendGrid (Email)         │
   │  - PDFKit (Generation)      │
   └─────────────────────────────┘
   ```

2. **Component Breakdown (4 hours)**
   ```typescript
   // Module structure
   src/
   ├── modules/
   │   ├── auth/
   │   ├── invoices/
   │   │   ├── invoices.module.ts
   │   │   ├── invoices.controller.ts
   │   │   ├── invoices.service.ts
   │   │   ├── dto/
   │   │   │   ├── create-invoice.dto.ts
   │   │   │   └── update-invoice.dto.ts
   │   │   └── entities/
   │   │       └── invoice.entity.ts
   │   ├── customers/
   │   ├── pdf/
   │   └── email/
   ├── common/
   │   ├── guards/
   │   ├── interceptors/
   │   └── pipes/
   └── prisma/
   ```

3. **Define API Contracts (4 hours)**
   ```typescript
   // All endpoints with request/response schemas
   
   // POST /invoices
   {
     "request": {
       "customerId": 1,
       "items": [
         { "description": "Consulting", "quantity": 10, "price": 100 }
       ],
       "dueDate": "2026-02-28"
     },
     "response": {
       "id": 1,
       "invoiceNumber": "INV-2026-001",
       "status": "draft",
       "total": 1000,
       "createdAt": "2026-01-19T..."
     }
   }
   ```

**Day 3: Database Design (8 hours)**

**Goals:**
- Create comprehensive database schema
- Define relationships
- Plan indexes

**Activities:**

1. **Prisma Schema (6 hours)**
   ```prisma
   model Customer {
     id        Int       @id @default(autoincrement())
     name      String
     email     String    @unique
     phone     String?
     address   String?
     invoices  Invoice[]
     createdAt DateTime  @default(now())
     updatedAt DateTime  @updatedAt
   }

   model Invoice {
     id            Int           @id @default(autoincrement())
     invoiceNumber String        @unique
     customer      Customer      @relation(fields: [customerId], references: [id])
     customerId    Int
     status        InvoiceStatus @default(DRAFT)
     items         InvoiceItem[]
     subtotal      Decimal       @db.Decimal(10, 2)
     tax           Decimal       @db.Decimal(10, 2)
     total         Decimal       @db.Decimal(10, 2)
     notes         String?
     dueDate       DateTime
     paidAt        DateTime?
     pdfUrl        String?
     createdBy     Int
     createdAt     DateTime      @default(now())
     updatedAt     DateTime      @updatedAt

     @@index([customerId])
     @@index([status])
     @@index([createdAt])
   }

   model InvoiceItem {
     id          Int     @id @default(autoincrement())
     invoice     Invoice @relation(fields: [invoiceId], references: [id], onDelete: Cascade)
     invoiceId   Int
     description String
     quantity    Int
     unitPrice   Decimal @db.Decimal(10, 2)
     total       Decimal @db.Decimal(10, 2)

     @@index([invoiceId])
   }

   enum InvoiceStatus {
     DRAFT
     SENT
     PAID
     OVERDUE
     CANCELLED
   }
   ```

2. **Optimization Planning (2 hours)**
   - Identify frequently queried fields → add indexes
   - Plan for pagination
   - Consider caching strategies

**Day 4-5: Implementation Planning & Presentation (16 hours)**

**Goals:**
- Break down into tasks
- Estimate effort
- Create sprint plan
- Present to team

**Activities:**

1. **Task Breakdown (6 hours)**
   ```csv
   Task,Description,Estimate (hours),Dependencies,Priority
   Setup project structure,Create modules and folders,2,,High
   Database schema,Implement Prisma schema and migrations,3,,High
   Customer CRUD,Basic customer management,4,Database schema,High
   Invoice creation,Create invoice with items,6,Customer CRUD,High
   PDF generation,Generate PDF from template,8,Invoice creation,High
   Email service,Send invoice via email,4,PDF generation,Medium
   Invoice status,Track and update status,3,Invoice creation,Medium
   Invoice history,View past invoices,3,Invoice creation,Low
   Testing,Write comprehensive tests,10,All features,High
   Documentation,API docs and README,4,All features,Medium
   ```

2. **Sprint Planning (4 hours)**
   ```json
   {
     "week_5": [
       "Setup project structure",
       "Database schema",
       "Customer CRUD",
       "Invoice creation"
     ],
     "week_6": [
       "PDF generation",
       "Email service",
       "Invoice status",
       "Invoice history",
       "Testing"
     ]
   }
   ```

3. **Risk Assessment (2 hours)**
   ```csv
   Risk,Likelihood,Impact,Mitigation
   PDF generation complexity,High,High,Use well-tested library (PDFKit); create simple template first
   Email delivery failures,Medium,Medium,Implement retry logic; log failures
   Scope creep,High,High,Stick to MVP; document "should have" for future
   Performance with large invoices,Low,Medium,Pagination; optimize queries
   ```

4. **Presentation Preparation (4 hours)**
   - Create slides (15-20 slides)
   - Practice demo
   - Prepare for Q&A
   - Include:
     - Problem statement
     - Solution overview
     - Architecture diagrams
     - Database schema
     - API endpoints
     - Timeline
     - Risks

**Deliverable:** Complete technical specification + team presentation

---

### ✅ Phase 2 Assessment

```csv
Deliverable,Description,Weight,Criteria
Project Proposal,Problem statement + solution approach,25%,Clear problem definition; realistic scope; measurable success criteria
Technical Spec,Architecture + database + API design,30%,Comprehensive design; scalable architecture; well-thought-out schema
Implementation Plan,Task breakdown + estimates + risks,20%,Detailed tasks; realistic estimates; risks identified
Presentation,Proposal presentation to team,25%,Clear communication; handles Q&A well; demonstrates understanding
```

**Evaluation:**
- **Proposal Quality**: Is the problem real and worth solving?
- **Technical Depth**: Is the design sound and scalable?
- **Feasibility**: Can this be built in 4 weeks?
- **Presentation**: Can you communicate technical ideas clearly?

**Passing Score:** 80%

---

### ⏰ Time Allocation

| Activity | Week 3 | Week 4 |
|----------|--------|--------|
| Research & interviews | 15h | 5h |
| Design & documentation | 15h | 20h |
| Feedback sessions | 5h | 8h |
| Presentation prep | 5h | 7h |

### 📖 Learning Resources

- [System Design Primer](https://github.com/donnemartin/system-design-primer) - Architecture patterns
- [Database Design Best Practices](https://www.vertabelo.com/blog/database-design-best-practices/)
- [Technical Specification Templates](https://www.toptal.com/freelance/why-design-documents-matter)
- [User Story Mapping](https://www.jpattonassociates.com/user-story-mapping/)

---



---

## PHASE 2: Final Project Phase (Weeks 5-8)

**Objective:** Build a substantial project applying all learned concepts

### Project Selection

At the end of Week 4, choose (with mentor guidance) ONE of these options:

#### 🏢 Option A: Internal System (Mid-Size Tool)

**Best for:** Building production tools used by the team

**Scope:** More complex than 1-month, includes advanced features

**What you'll build:**
- Developer productivity tool (e.g., deployment dashboard)
- Data analytics platform with visualizations
- Workflow automation system
- Multi-tenant admin portal
- Team collaboration tool

**Requirements:**
- 8-12 API endpoints
- Authentication + role-based authorization
- Database with 5+ related tables
- Background jobs or scheduled tasks
- Real-time features (WebSockets optional)
- Admin interface integration

**Deliverables:**
- Complete system architecture document
- Deployed to internal infrastructure
- API documentation (Swagger/Postman)
- 80%+ test coverage
- User guide and deployment docs
- Stakeholder demo + handover

---

#### 🌍 Option B: Real-World Application (Major Feature)

**Best for:** Serious production experience with complex features

**Scope:** Own a significant feature or module

**What you'll contribute:**
- Complete authentication/authorization module
- Payment integration system
- File upload and processing pipeline
- Search functionality with Elasticsearch
- Notification system (email + push)
- Reporting/analytics feature

**Requirements:**
- 5-8 production-ready endpoints
- Integration with existing systems
- Comprehensive error handling
- Performance optimization
- Security best practices
- Production monitoring setup

**Deliverables:**
- 5+ merged and deployed pull requests
- Technical design document
- All code reviewed and approved
- Tests for all new code (80%+ coverage)
- Production deployment completed
- Post-deployment monitoring report
- Knowledge transfer session

---

#### 🚀 Option C: New Application (Complete Platform)

**Best for:** Full ownership and entrepreneurial experience

**Scope:** Production-ready application from scratch

**What you'll create:**
- Multi-tenant SaaS backend
- Marketplace platform API
- Content management system
- Social network backend
- Booking/scheduling system
- E-commerce platform

**Requirements:**
- 10-15 comprehensive endpoints
- Complete authentication system (JWT + refresh tokens)
- Role-based access control (RBAC)
- Database with complex relations
- File upload to cloud storage
- Email notifications
- API rate limiting
- Caching layer (Redis)
- Background job processing
- Full deployment with CI/CD

**Deliverables:**
- System architecture diagram
- Complete API with documentation
- Database migrations and seeding
- Frontend integration guide
- Deployed to cloud (AWS/GCP/Azure)
- Custom domain with HTTPS
- Monitoring and logging setup
- Comprehensive README
- API usage examples
- Final presentation + demo

---

### Weeks 5-6: Core Development & Testing

**Time allocation:** 80 hours (2 weeks × 40 hours)

```
Week 5: Foundation & Core Features
├─ Days 1-2 (16h): Project setup & architecture
│  ├─ Initialize repository
│  ├─ Set up project structure
│  ├─ Configure CI/CD pipeline
│  ├─ Design database schema
│  └─ Create initial migrations
│
├─ Days 3-5 (24h): Core feature implementation
│  ├─ Implement main entities
│  ├─ Build primary API endpoints
│  ├─ Add validation layers
│  ├─ Implement business logic
│  └─ Write unit tests

Week 6: Advanced Features & Testing
├─ Days 1-2 (16h): Advanced features
│  ├─ Authentication/authorization
│  ├─ Complex business logic
│  ├─ Third-party integrations
│  └─ Background jobs (if needed)
│
├─ Days 3-4 (16h): Comprehensive testing
│  ├─ Integration tests
│  ├─ E2E tests for critical flows
│  ├─ Security testing
│  └─ Performance testing
│
└─ Day 5 (8h): Code review & refinement
   ├─ Self code review
   ├─ Mentor review
   ├─ Address feedback
   └─ Refactor as needed
```

**Milestone:** Feature complete with 80%+ test coverage

---

### Weeks 7-8: Deployment & Presentation

**Time allocation:** 80 hours (2 weeks × 40 hours)

```
Week 7: Production Deployment
├─ Days 1-2 (16h): Infrastructure setup
│  ├─ Configure production environment
│  ├─ Set up database (managed service)
│  ├─ Configure environment variables
│  ├─ Set up Redis/caching (if needed)
│  └─ Domain and SSL setup
│
├─ Days 3-4 (16h): CI/CD & deployment
│  ├─ Create deployment pipeline
│  ├─ Configure auto-deploy on merge
│  ├─ Set up database migrations
│  ├─ Deploy to staging environment
│  └─ Integration testing on staging
│
└─ Day 5 (8h): Production deployment
   ├─ Deploy to production
   ├─ Run smoke tests
   ├─ Monitor for issues
   └─ Verify all features work

Week 8: Documentation & Presentation
├─ Days 1-2 (16h): Documentation
│  ├─ API documentation (Swagger/OpenAPI)
│  ├─ Setup/deployment guide
│  ├─ Architecture decision records (ADRs)
│  ├─ Runbook for operations
│  └─ User guide (if applicable)
│
├─ Days 3-4 (16h): Monitoring & polish
│  ├─ Set up application monitoring (Sentry/Datadog)
│  ├─ Configure log aggregation
│  ├─ Set up alerts for errors
│  ├─ Performance optimization
│  └─ Security hardening
│
└─ Day 5 (8h): Final presentation
   ├─ Prepare demo slides
   ├─ Record demo video
   ├─ Present to stakeholders
   ├─ Q&A session
   └─ Gather feedback
```

**Final Deliverable:** Production-deployed project with full documentation

---

## Success Criteria

To successfully complete this 2-month program:

### Technical Requirements
- ✅ Complete all Week 1-4 learning modules
- ✅ Build authentication system (Week 1-2 project)
- ✅ Create approved project proposal (Week 3-4)
- ✅ Complete chosen final project with all deliverables
- ✅ Write comprehensive tests (minimum 80% coverage)
- ✅ Deploy to production environment
- ✅ Set up monitoring and alerting

### Project-Specific Criteria

**For Internal System (Option A):**
- ✅ 8-12 working endpoints
- ✅ Role-based access control
- ✅ Deployed internally with monitoring
- ✅ Stakeholder approval and adoption

**For Real-World App (Option B):**
- ✅ 5+ merged PRs in production
- ✅ Pass all code review standards
- ✅ Zero production bugs introduced
- ✅ Performance meets SLAs

**For New Application (Option C):**
- ✅ 10-15 working endpoints
- ✅ Custom domain with HTTPS
- ✅ CI/CD pipeline functional
- ✅ Monitoring and logging active
- ✅ Complete API documentation

### Soft Skills
- ✅ Proactive communication
- ✅ Regular status updates
- ✅ Effective time management
- ✅ Handle feedback professionally
- ✅ Strong presentation skills

---

## Assessment & Evaluation

Evaluated across three dimensions:

1. **Technical Skills (40%)**
   - Advanced code quality
   - System design decisions
   - Testing strategy
   - Security implementation
   - Performance considerations

2. **Deliverables (30%)**
   - Project completion
   - Documentation quality
   - Production deployment
   - Monitoring setup

3. **Soft Skills (30%)**
   - Leadership and ownership
   - Communication effectiveness
   - Problem-solving approach
   - Time management
   - Presentation quality

See [Evaluation Criteria](./evaluation.md) for detailed rubrics.

---

## What Happens After?

Upon successful completion:

### Immediate Outcomes
1. **Certificate of Completion** - Official program credential
2. **Detailed Performance Review** - Written feedback
3. **Portfolio Project** - Production-ready case study
4. **LinkedIn Endorsement** - From mentor and stakeholders
5. **Reference Letter** - For job applications

### Career Opportunities

**Strong performers typically receive:**
- **Mid-level Developer offers** - Direct hire opportunities
- **Contract work** - 3-6 month contracts
- **Lead roles in 3-month program** - Mentor junior interns
- **Referrals** - To partner companies
- **Freelance projects** - Ongoing opportunities

### Continued Growth Paths

1. **Join as Full-Time Developer**
   - Mid-level backend role
   - Competitive salary package
   - Full benefits

2. **Extend to 3-Month Program** (if not hired immediately)
   - Build more advanced project
   - Gain additional experience
   - Strengthen portfolio

3. **External Opportunities**
   - Strong portfolio to show employers
   - Reference from senior engineers
   - Network in the industry

See [Career Paths](./career-paths.md) for detailed guidance.

---

## Tips for Success

### Learning Phase (Weeks 1-4)
```
DO:
├─ Build everything yourself (no copy-paste)
├─ Ask "why" not just "how"
├─ Read production code examples
├─ Practice system design thinking
└─ Start thinking about project ideas early

AVOID:
├─ Rushing through fundamentals
├─ Skipping the practice projects
├─ Copying solutions without understanding
└─ Postponing project planning to Week 4
```

### Project Phase (Weeks 5-8)
```
DO:
├─ Start simple, add complexity gradually
├─ Commit and push code daily
├─ Write tests as you develop
├─ Ask for code reviews early and often
├─ Document as you build
└─ Deploy to staging frequently

AVOID:
├─ Over-engineering solutions
├─ Feature creep (stick to core requirements)
├─ Leaving deployment to the last week
├─ Skipping security considerations
└─ Ignoring performance implications
```

---

## Frequently Asked Questions

**Q: Is this harder than the 1-month program?**  
A: Yes, significantly. You need strong fundamentals before starting. Complete 1-month if you're unsure.

**Q: Can I build my startup idea as the final project?**  
A: Yes! That's Option C. Ensure it demonstrates all required technical skills.

**Q: What if my project is too ambitious?**  
A: Your mentor will help scope it appropriately in Week 3-4. Better to complete 80% of a focused project than 40% of an ambitious one.

**Q: Do I need DevOps experience?**  
A: Basic knowledge helps, but we'll teach you. Focus on backend logic first.

**Q: Can I use different tools than the core stack?**  
A: With justification and approval. E.g., using Redis for caching is fine. Switching from Prisma to TypeORM needs strong reasoning.

**Q: What if I fall behind schedule?**  
A: Communicate immediately. We can adjust project scope or extend timeline slightly.

See [Full FAQs](./faqs.md) for more questions.

---

## Ready to Start?

### Prerequisites
- [ ] Complete [Prerequisites Guide](./prerequisites.md)
- [ ] Have 1+ year programming experience OR completed 1-month program
- [ ] Comfortable with Git, basic databases, and APIs
- [ ] Can dedicate 40 hours/week for 2 months
- [ ] Have project ideas in mind

### First Week Preparation
1. **Review fundamentals** (if rusty)
2. **Set up environment** completely
3. **Join program channels** (Slack/Discord)
4. **Meet your mentor** - Schedule kickoff
5. **Review example projects** from [Success Stories](./success-stories.md)

---

## Navigation
[← Back to Overview](./overview.md) | [← 1-Month Program](./1-month-program.md) | [Next: 3-Month Program →](./3-month-program.md)

**Related:** [Skills Matrix](./skills-matrix.md) | [Evaluation Criteria](./evaluation.md) | [Success Stories](./success-stories.md)

---

**Questions?** Contact your mentor or check [FAQs](./faqs.md)

**Need help?** See [Quick Reference](./appendix.md) for commands and troubleshooting
