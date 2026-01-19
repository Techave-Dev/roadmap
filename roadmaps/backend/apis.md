---
sidebar_position: 4
---

# APIs & Web Services

Master API design and implementation with REST, GraphQL, and modern patterns.

## What is an API?

An **API (Application Programming Interface)** allows different software systems to communicate with each other. Backend APIs expose endpoints that clients (web, mobile, desktop) can call to interact with your application.

---

## REST APIs

**REST (Representational State Transfer)** is the most common API architecture for web services.

### Core Principles

1. **Stateless**: Each request contains all necessary information
2. **Client-Server**: Separation of concerns
3. **Cacheable**: Responses should define caching rules
4. **Uniform Interface**: Consistent resource identification and manipulation
5. **Layered System**: Client doesn't know if it's talking to the end server or an intermediary

### HTTP Methods

| Method | Purpose | Idempotent | Safe |
|--------|---------|-----------|------|
| **GET** | Retrieve data | ✅ | ✅ |
| **POST** | Create new resource | ❌ | ❌ |
| **PUT** | Update/replace entire resource | ✅ | ❌ |
| **PATCH** | Partially update resource | ❌ | ❌ |
| **DELETE** | Remove resource | ✅ | ❌ |

### RESTful URL Design

**Good Practices:**
```
GET    /api/users              # List all users
GET    /api/users/123          # Get specific user
POST   /api/users              # Create new user
PUT    /api/users/123          # Update entire user
PATCH  /api/users/123          # Partially update user
DELETE /api/users/123          # Delete user

GET    /api/users/123/posts    # Get user's posts
POST   /api/users/123/posts    # Create post for user
```

**Bad Practices:**
```
❌ GET  /api/getUsers
❌ POST /api/createUser
❌ GET  /api/user?action=delete
❌ GET  /api/users/deleteUser/123
```

### HTTP Status Codes

**2xx Success:**
- `200 OK` - Request succeeded
- `201 Created` - Resource created successfully
- `204 No Content` - Success but no response body

**3xx Redirection:**
- `301 Moved Permanently` - Resource moved
- `304 Not Modified` - Cached version is still valid

**4xx Client Errors:**
- `400 Bad Request` - Invalid request format
- `401 Unauthorized` - Authentication required
- `403 Forbidden` - Authenticated but not allowed
- `404 Not Found` - Resource doesn't exist
- `422 Unprocessable Entity` - Validation failed
- `429 Too Many Requests` - Rate limit exceeded

**5xx Server Errors:**
- `500 Internal Server Error` - Something went wrong
- `502 Bad Gateway` - Invalid response from upstream
- `503 Service Unavailable` - Server temporarily down

### Request/Response Format

**Request Example:**
```http
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{
  "name": "John Doe",
  "email": "john@example.com",
  "role": "user"
}
```

**Response Example:**
```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/users/123

{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "role": "user",
  "createdAt": "2024-01-19T10:30:00Z"
}
```

### Pagination

**Offset-based:**
```
GET /api/users?page=2&limit=20
```

**Cursor-based (recommended for large datasets):**
```
GET /api/users?cursor=eyJpZCI6MTIzfQ==&limit=20
```

**Response:**
```json
{
  "data": [...],
  "pagination": {
    "nextCursor": "eyJpZCI6MTQzfQ==",
    "hasMore": true,
    "total": 1500
  }
}
```

### Filtering, Sorting, Searching

```
GET /api/products?category=electronics&minPrice=100&sort=-price&search=laptop
```

### Versioning

**URL Versioning (most common):**
```
GET /api/v1/users
GET /api/v2/users
```

**Header Versioning:**
```
GET /api/users
Accept-Version: v1
```

---

## GraphQL

**GraphQL** is a query language for APIs that lets clients request exactly the data they need.

### Core Concepts

1. **Schema**: Defines data structure and operations
2. **Queries**: Fetch data (like GET)
3. **Mutations**: Modify data (like POST/PUT/DELETE)
4. **Subscriptions**: Real-time updates

### Schema Example

```graphql
type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  content: String!
  author: User!
  createdAt: DateTime!
}

type Query {
  user(id: ID!): User
  users(limit: Int, offset: Int): [User!]!
  post(id: ID!): Post
}

type Mutation {
  createUser(name: String!, email: String!): User!
  updateUser(id: ID!, name: String, email: String): User!
  deleteUser(id: ID!): Boolean!
}

type Subscription {
  postCreated: Post!
}
```

### Query Example

**Request:**
```graphql
query {
  user(id: "123") {
    id
    name
    email
    posts {
      id
      title
      createdAt
    }
  }
}
```

**Response:**
```json
{
  "data": {
    "user": {
      "id": "123",
      "name": "John Doe",
      "email": "john@example.com",
      "posts": [
        {
          "id": "1",
          "title": "My First Post",
          "createdAt": "2024-01-15T10:00:00Z"
        }
      ]
    }
  }
}
```

### Mutation Example

```graphql
mutation {
  createUser(name: "Jane Doe", email: "jane@example.com") {
    id
    name
    email
  }
}
```

### When to Use GraphQL

**Advantages:**
- Clients request exactly what they need (no over/under-fetching)
- Single endpoint for all queries
- Strong typing
- Great for complex, nested data
- Excellent developer tooling (GraphiQL, Apollo Studio)

**Disadvantages:**
- More complex to implement
- Caching is harder
- Overkill for simple APIs
- Learning curve for team

**Use GraphQL when:**
- You have complex, nested data relationships
- Multiple client types (web, mobile) with different data needs
- You want to reduce the number of endpoints
- You need flexible querying

**Use REST when:**
- Simple CRUD operations
- Standard caching works well
- Team is familiar with REST
- Public API for third parties

---

## API Design Best Practices

### 1. Consistency

- Use consistent naming conventions (snake_case or camelCase)
- Predictable URL patterns
- Standard response formats

### 2. Error Handling

**Good Error Response:**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid user data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      },
      {
        "field": "password",
        "message": "Password must be at least 8 characters"
      }
    ]
  }
}
```

### 3. Security

- **HTTPS Only**: Encrypt all traffic
- **Authentication**: Require valid credentials
- **Authorization**: Check permissions
- **Rate Limiting**: Prevent abuse
- **Input Validation**: Sanitize all inputs
- **CORS**: Configure allowed origins

### 4. Documentation

- **OpenAPI/Swagger**: Auto-generate API docs
- **Examples**: Provide request/response samples
- **Postman Collections**: Share API collections
- **GraphQL Schema**: Self-documenting

### 5. Performance

- **Caching**: Use ETags, Cache-Control headers
- **Compression**: Gzip/Brotli responses
- **Pagination**: Don't return huge datasets
- **Selective Fields**: Allow clients to specify fields
- **Database Optimization**: Index frequently queried fields

---

## API Testing

### Tools:
- **Postman** - GUI-based API testing
- **Insomnia** - Alternative to Postman
- **curl** - Command-line HTTP client
- **Thunder Client** - VS Code extension
- **HTTPie** - User-friendly CLI tool

### Testing Strategies:

1. **Unit Tests**: Test individual functions
2. **Integration Tests**: Test API endpoints
3. **Load Tests**: Test under high traffic (JMeter, k6)
4. **Contract Tests**: Ensure API matches spec (Pact)

---

## Rate Limiting

Protect your API from abuse:

```javascript
// Example: 100 requests per 15 minutes per IP
app.use(rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  message: 'Too many requests from this IP'
}));
```

---

## Webhooks

Allow clients to register URLs to receive event notifications:

```javascript
// When an event occurs, POST to registered webhook URLs
POST https://client.example.com/webhooks/payment-completed
Content-Type: application/json

{
  "event": "payment.completed",
  "data": {
    "paymentId": "pay_123",
    "amount": 50.00,
    "currency": "USD"
  },
  "timestamp": "2024-01-19T10:30:00Z"
}
```

---

## Real-Time APIs

### WebSockets

Bi-directional communication for real-time features:

```javascript
// Client
const socket = new WebSocket('wss://api.example.com/chat');
socket.send(JSON.stringify({ message: 'Hello!' }));
socket.onmessage = (event) => console.log(event.data);

// Server (Node.js)
wss.on('connection', (ws) => {
  ws.on('message', (data) => {
    // Broadcast to all clients
    wss.clients.forEach(client => client.send(data));
  });
});
```

### Server-Sent Events (SSE)

One-way updates from server to client:

```javascript
// Client
const eventSource = new EventSource('/api/events');
eventSource.onmessage = (event) => {
  console.log('New message:', event.data);
};

// Server
res.setHeader('Content-Type', 'text/event-stream');
res.write(`data: ${JSON.stringify({ message: 'Hello' })}\n\n`);
```

---

## API Gateways

Centralized entry point for microservices:

**Features:**
- Routing and load balancing
- Authentication and authorization
- Rate limiting
- Request/response transformation
- Caching
- Logging and monitoring

**Popular Gateways:**
- **Kong**
- **AWS API Gateway**
- **Azure API Management**
- **Traefik**
- **NGINX**

---

## Learning Path

1. **Build a simple REST API**
   - CRUD endpoints for one resource
   - Proper HTTP methods and status codes
   - Input validation and error handling

2. **Add complexity**
   - Relationships between resources
   - Pagination, filtering, sorting
   - Authentication and authorization

3. **Implement best practices**
   - API documentation (Swagger)
   - Rate limiting
   - Caching strategies
   - Comprehensive testing

4. **Explore GraphQL**
   - Build schema and resolvers
   - Compare with your REST API
   - Implement subscriptions

5. **Real-time features**
   - WebSockets for chat/notifications
   - Server-Sent Events for updates

## Practice Projects

1. **Todo API** - Simple CRUD with REST
2. **Blog API** - Posts, comments, nested resources
3. **E-commerce API** - Products, orders, cart
4. **Social Media API** - Users, posts, likes, followers
5. **Real-time Chat API** - WebSockets for messaging
6. **GraphQL API** - Rebuild one of the above with GraphQL

## Next Steps

Once you're comfortable with APIs:
- **Authentication**: Secure your APIs with JWT, OAuth
- **Architecture**: Design scalable microservices
- **Deployment**: Deploy APIs to production
