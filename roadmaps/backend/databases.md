---
sidebar_position: 3
---

# Databases

Master data storage and retrieval with SQL and NoSQL databases.

## Database Types

### Relational Databases (SQL)

Store data in tables with predefined schemas. Best for structured data with complex relationships.

**When to Use:**
- Complex queries with JOINs
- ACID compliance needed (banking, finance)
- Structured data with clear relationships
- Data integrity is critical

### NoSQL Databases

Flexible schema designs for various data models. Best for unstructured or rapidly changing data.

**When to Use:**
- Massive scale (millions of users)
- Flexible/changing data structures
- High-speed reads/writes
- Specific use cases (caching, real-time, graphs)

---

## SQL Databases

### 1. PostgreSQL

**Best for:** Complex queries, data integrity, advanced features

**Strengths:**
- ACID compliant
- Advanced features (JSON, full-text search, arrays)
- Excellent for complex queries
- Strong data integrity
- Open source

**Use Cases:**
- E-commerce applications
- Financial systems
- Analytical applications
- Geospatial data (PostGIS)

**Key Concepts:**
- Tables, rows, columns
- Primary and foreign keys
- Indexes
- Transactions
- Views and materialized views
- Stored procedures

**Learning Resources:**
- [PostgreSQL Official Tutorial](https://www.postgresql.org/docs/current/tutorial.html)
- [PostgreSQL Exercises](https://pgexercises.com/)

---

### 2. MySQL / MariaDB

**Best for:** Web applications, read-heavy workloads

**Strengths:**
- Fast read performance
- Widely supported
- Easy to set up
- Large community
- Good for web apps

**Use Cases:**
- WordPress/CMS platforms
- Web applications
- Content management
- Small to medium applications

**Learning Resources:**
- [MySQL Tutorial](https://www.mysqltutorial.org/)
- [MySQL Official Docs](https://dev.mysql.com/doc/)

---

### 3. SQLite

**Best for:** Embedded databases, mobile apps, local storage

**Strengths:**
- Serverless, zero-configuration
- Single file database
- Great for development/testing
- Perfect for mobile apps
- No installation needed

**Use Cases:**
- Mobile applications (iOS, Android)
- Desktop applications
- Local development
- Embedded systems

**Learning Resources:**
- [SQLite Tutorial](https://www.sqlitetutorial.net/)
- [SQLite Official Docs](https://www.sqlite.org/docs.html)

---

## Essential SQL Skills

### 1. Basic Queries

```sql
-- SELECT data
SELECT id, name, email FROM users WHERE active = true;

-- INSERT data
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');

-- UPDATE data
UPDATE users SET email = 'newemail@example.com' WHERE id = 1;

-- DELETE data
DELETE FROM users WHERE id = 1;
```

### 2. JOINs

```sql
-- INNER JOIN
SELECT orders.id, users.name, orders.total
FROM orders
INNER JOIN users ON orders.user_id = users.id;

-- LEFT JOIN
SELECT users.name, orders.total
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

### 3. Aggregations

```sql
-- COUNT, SUM, AVG
SELECT COUNT(*) FROM users;
SELECT SUM(total) FROM orders WHERE user_id = 1;
SELECT AVG(price) FROM products;

-- GROUP BY
SELECT user_id, COUNT(*) as order_count
FROM orders
GROUP BY user_id
HAVING COUNT(*) > 5;
```

### 4. Indexes

```sql
-- Create index for faster queries
CREATE INDEX idx_email ON users(email);

-- Unique index
CREATE UNIQUE INDEX idx_username ON users(username);
```

### 5. Transactions

```sql
-- Ensure data consistency
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
-- Or ROLLBACK; if something went wrong
```

---

## NoSQL Databases

### 1. MongoDB (Document Database)

**Best for:** Flexible schemas, rapid development, JSON-like data

**Strengths:**
- Flexible document model
- Easy to scale horizontally
- Great for prototyping
- Native JSON support
- Rich query language

**Use Cases:**
- Content management systems
- Real-time analytics
- IoT applications
- Catalogs and product data

**Key Concepts:**
- Collections (like tables)
- Documents (like rows, but nested)
- Embedded documents vs references
- Indexes
- Aggregation pipeline

**Example:**
```javascript
// Insert document
db.users.insertOne({
  name: "John Doe",
  email: "john@example.com",
  address: {
    city: "New York",
    zip: "10001"
  },
  interests: ["coding", "music"]
});

// Query with filters
db.users.find({ "address.city": "New York" });

// Update
db.users.updateOne(
  { email: "john@example.com" },
  { $set: { status: "active" } }
);
```

**Learning Resources:**
- [MongoDB University](https://university.mongodb.com/)
- [MongoDB Manual](https://docs.mongodb.com/manual/)

---

### 2. Redis (Key-Value Store / Cache)

**Best for:** Caching, real-time applications, sessions, pub/sub

**Strengths:**
- Extremely fast (in-memory)
- Multiple data structures (strings, lists, sets, hashes)
- Built-in pub/sub
- Atomic operations
- Persistence options

**Use Cases:**
- Session storage
- Caching layer
- Real-time leaderboards
- Rate limiting
- Message queues

**Key Concepts:**
- Key-value pairs
- Data expiration (TTL)
- Data structures (strings, hashes, lists, sets, sorted sets)
- Pub/sub messaging
- Transactions (MULTI/EXEC)

**Example:**
```bash
# Set and get values
SET user:1000:name "John Doe"
GET user:1000:name

# With expiration
SETEX session:abc123 3600 "user-data"

# Lists
LPUSH notifications "New message"
RPOP notifications

# Pub/Sub
PUBLISH chat:room1 "Hello everyone!"
```

**Learning Resources:**
- [Redis Documentation](https://redis.io/documentation)
- [Try Redis](https://try.redis.io/)

---

### 3. Elasticsearch (Search Engine)

**Best for:** Full-text search, log analytics, complex queries

**Strengths:**
- Powerful full-text search
- Fast search performance
- Distributed and scalable
- Real-time indexing
- Excellent for analytics

**Use Cases:**
- Search functionality
- Log and event data analysis
- Application monitoring
- Business analytics

**Learning Resources:**
- [Elasticsearch Guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

---

### 4. Cassandra (Wide-Column Store)

**Best for:** Time-series data, massive scale, high availability

**Strengths:**
- Linear scalability
- No single point of failure
- High write throughput
- Multi-datacenter support

**Use Cases:**
- Time-series data
- IoT sensor data
- Messaging platforms
- High-volume writes

---

## Database Design Principles

### 1. Normalization (SQL)

- **1NF**: Eliminate repeating groups
- **2NF**: Remove partial dependencies
- **3NF**: Remove transitive dependencies

### 2. Denormalization (NoSQL & Performance)

Sometimes duplicate data for faster reads (especially in NoSQL).

### 3. Indexing Strategies

- Index frequently queried fields
- Avoid over-indexing (slows writes)
- Use composite indexes for multi-field queries

### 4. Relationships

- **One-to-Many**: User has many posts
- **Many-to-Many**: Students and courses (join table)
- **One-to-One**: User has one profile

---

## Database Security

### Best Practices:

1. **Never store passwords in plain text** - Use bcrypt/argon2
2. **Use parameterized queries** - Prevent SQL injection
3. **Principle of least privilege** - Minimal database permissions
4. **Encrypt sensitive data** - At rest and in transit
5. **Regular backups** - Automated and tested
6. **Connection pooling** - Reuse database connections
7. **Environment variables** - Never hardcode credentials

---

## ORMs and Query Builders

### Node.js:
- **Prisma** - Modern, type-safe ORM
- **TypeORM** - Supports multiple databases
- **Sequelize** - Mature, feature-rich
- **Knex.js** - SQL query builder

### Python:
- **SQLAlchemy** - Powerful ORM
- **Django ORM** - Built into Django
- **Peewee** - Lightweight ORM

### Java:
- **Hibernate** - Industry standard
- **JPA** - Specification for ORMs
- **jOOQ** - Type-safe SQL builder

---

## Learning Path

1. **Start with SQL basics**
   - Learn SELECT, INSERT, UPDATE, DELETE
   - Practice JOINs and relationships
   - Understand indexes and transactions

2. **Master one SQL database**
   - PostgreSQL recommended
   - Build projects with real schemas
   - Learn optimization techniques

3. **Explore NoSQL**
   - Learn MongoDB for documents
   - Redis for caching
   - Understand when to use each type

4. **Learn database design**
   - Schema design patterns
   - Normalization vs denormalization
   - Performance optimization

5. **Use ORMs/Query Builders**
   - Learn Prisma/SQLAlchemy/Hibernate
   - Understand migrations
   - Handle relationships properly

## Practice Projects

1. **User Management System** - CRUD with PostgreSQL
2. **Blog Platform** - Posts, comments, relationships
3. **E-commerce Database** - Products, orders, inventory
4. **Caching Layer** - Add Redis to optimize reads
5. **Search Feature** - Implement full-text search
6. **Analytics Dashboard** - Aggregations and reports

## Next Steps

Once you're comfortable with databases:
- **APIs**: Build endpoints that interact with databases
- **Authentication**: Store and validate user credentials securely
- **Architecture**: Design scalable data systems
