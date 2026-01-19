# Prerequisites

Everything you need before starting the internship program.

## Navigation
[← Back to Overview](./overview.md) | [Tech Stack Details →](./tech-stack.md)

---

## Required Skills (Before Starting)

### Must Have

```csv
Skill,Level,Why It's Important,How to Verify
Basic Programming,Beginner,Foundation for learning TypeScript,Can write simple programs in any language
Web Concepts,Basic,Understanding HTTP APIs,Know what HTTP GET/POST means
Command Line,Basic,Running commands installing tools,Can navigate directories run basic commands
Git Basics,Basic,Version control collaboration,Can commit push pull
Basic SQL,Basic,Database fundamentals,Can write SELECT WHERE queries
```

#### 1. Basic Programming Knowledge (Any Language)

**What you should know:**
```javascript
// Variables and data types
const name = "John";
const age = 25;
const isActive = true;

// Functions
function greet(name) {
  return `Hello, ${name}!`;
}

// Loops
for (let i = 0; i < 10; i++) {
  console.log(i);
}

// Conditionals
if (age >= 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}

// Arrays and objects
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];
```

**Self-Test:**
- ✅ Can you write a function that takes an array of numbers and returns the sum?
- ✅ Can you filter an array to only include even numbers?
- ✅ Can you create an object and access its properties?

**Don't have this?**
- [freeCodeCamp JavaScript](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/) - 300 hours free
- [Codecademy Learn JavaScript](https://www.codecademy.com/learn/introduction-to-javascript) - Interactive

---

#### 2. Web Concepts (HTTP, APIs)

**What you should know:**
- What is HTTP?
- What are HTTP methods (GET, POST, PUT, DELETE)?
- What is an API?
- What is JSON?
- What are HTTP status codes (200, 404, 500)?

**Examples:**
```
# HTTP Request
GET https://api.example.com/users/123
Headers:
  Authorization: Bearer token123

# HTTP Response
Status: 200 OK
Body:
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Self-Test:**
- ✅ Can you explain what a REST API is?
- ✅ Do you know the difference between GET and POST?
- ✅ Can you read a JSON response?

**Resources:**
- [HTTP Basics](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [REST API Tutorial](https://restfulapi.net/)

---

#### 3. Command Line Basics

**What you should know:**
```bash
# Navigation
cd /path/to/directory    # Change directory
pwd                      # Print working directory
ls                       # List files

# File operations
mkdir my-folder          # Create directory
touch file.txt           # Create file
rm file.txt              # Delete file
cp source.txt dest.txt   # Copy file
mv old.txt new.txt       # Move/rename file

# Viewing files
cat file.txt             # Display file content
less file.txt            # Page through file

# Package managers
npm install package-name # Install Node package
```

**Self-Test:**
- ✅ Can you navigate to your home directory from anywhere?
- ✅ Can you create a folder and create a file inside it?
- ✅ Can you view the contents of a file?

**Resources:**
- [Command Line Crash Course](https://www.freecodecamp.org/news/command-line-for-beginners/)
- [Linux Command Line Basics](https://ubuntu.com/tutorials/command-line-for-beginners)

---

#### 4. Git Basics

**What you should know:**
```bash
# Setup
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# Basic workflow
git init                 # Initialize repository
git add file.txt         # Stage file
git add .                # Stage all files
git commit -m "message"  # Commit changes
git status               # Check status
git log                  # View history

# Branches
git branch feature-name  # Create branch
git checkout feature-name # Switch to branch
git checkout -b new-branch # Create and switch

# Remote
git clone url            # Clone repository
git push origin main     # Push to remote
git pull origin main     # Pull from remote
```

**Self-Test:**
- ✅ Can you clone a repository?
- ✅ Can you make changes, commit, and push?
- ✅ Can you create a new branch?

**Resources:**
- [Git Tutorial](https://www.atlassian.com/git/tutorials)
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive
- [GitHub Skills](https://skills.github.com/) - Hands-on tutorials

---

#### 5. Basic SQL

**What you should know:**
```sql
-- Select data
SELECT * FROM users;
SELECT name, email FROM users WHERE age > 18;

-- Filter and sort
SELECT * FROM users WHERE city = 'New York' ORDER BY name ASC;

-- Count and aggregate
SELECT COUNT(*) FROM users;
SELECT city, COUNT(*) as user_count FROM users GROUP BY city;

-- Simple join
SELECT posts.title, users.name 
FROM posts 
JOIN users ON posts.user_id = users.id;

-- Insert, update, delete
INSERT INTO users (name, email) VALUES ('John', 'john@example.com');
UPDATE users SET email = 'new@example.com' WHERE id = 1;
DELETE FROM users WHERE id = 1;
```

**Self-Test:**
- ✅ Can you write a SELECT query with a WHERE clause?
- ✅ Can you join two tables?
- ✅ Can you insert and update records?

**Resources:**
- [SQL Tutorial](https://www.sqltutorial.org/)
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)
- [SQLBolt](https://sqlbolt.com/) - Interactive exercises

---

### Recommended (Nice to Have)

These are not required but will help you learn faster:

```csv
Skill,Benefit
JavaScript Experience,TypeScript is a superset of JavaScript
Understanding of REST APIs,You'll build REST APIs
Basic database concepts,Helps with Prisma and PostgreSQL
Linux/Unix basics,Most servers run Linux
```

---

## Technical Setup

### Required Software

```csv
Category,Item,Version,Required,Download Link,Install Time
Language,Node.js,v20+,Yes,https://nodejs.org,5 min
Editor,VS Code,Latest,Yes,https://code.visualstudio.com,5 min
Version Control,Git,Latest,Yes,https://git-scm.com,5 min
Container,Docker Desktop,Latest,Yes,https://www.docker.com/products/docker-desktop,10 min
Database,PostgreSQL,v16+,Yes,https://www.postgresql.org/download/,10 min
API Testing,Postman or Insomnia,Latest,Yes,https://www.postman.com,5 min
DB GUI,TablePlus or pgAdmin,Latest,Recommended,https://tableplus.com,5 min
```

### Installation Guide

#### 1. Node.js (JavaScript Runtime)

**macOS:**
```bash
# Using Homebrew
brew install node@20

# Verify
node --version  # Should show v20.x.x
npm --version   # Should show v10.x.x
```

**Windows:**
1. Download installer from [nodejs.org](https://nodejs.org/)
2. Run installer (choose "Automatically install necessary tools")
3. Restart terminal
4. Verify: `node --version`

**Linux (Ubuntu/Debian):**
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Verify
node --version
```

---

#### 2. VS Code (Code Editor)

**All Platforms:**
1. Download from [code.visualstudio.com](https://code.visualstudio.com/)
2. Install
3. Install recommended extensions:
   ```
   - ESLint
   - Prettier
   - Prisma
   - GitLens
   - Thunder Client (API testing)
   - Error Lens
   - Auto Rename Tag
   - Path Intellisense
   ```

**Configure Settings:**
```json
// settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

---

#### 3. Git (Version Control)

**macOS:**
```bash
brew install git
```

**Windows:**
1. Download from [git-scm.com](https://git-scm.com/)
2. Run installer (use default settings)

**Linux:**
```bash
sudo apt-get install git
```

**Configure:**
```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global init.defaultBranch main
```

---

#### 4. Docker Desktop (Containerization)

**Why?** Run PostgreSQL and other services without complex setup.

**All Platforms:**
1. Download from [docker.com](https://www.docker.com/products/docker-desktop)
2. Install and start Docker Desktop
3. Verify: `docker --version`

**Test Docker:**
```bash
docker run hello-world
```

---

#### 5. PostgreSQL (Database)

**Option A: Docker (Recommended)**
```bash
# Create docker-compose.yml
version: '3.8'
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: internship_db
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:

# Start
docker-compose up -d

# Stop
docker-compose down
```

**Option B: Native Install**

**macOS:**
```bash
brew install postgresql@16
brew services start postgresql@16
```

**Windows:**
1. Download from [postgresql.org](https://www.postgresql.org/download/windows/)
2. Run installer
3. Remember password you set!

**Linux:**
```bash
sudo apt-get install postgresql-16
sudo systemctl start postgresql
```

**Verify:**
```bash
psql --version  # Should show 16.x
```

---

#### 6. API Testing Tool

**Postman** (Recommended for beginners)
1. Download from [postman.com](https://www.postman.com/downloads/)
2. Install
3. Create account (free)

**Thunder Client** (VS Code Extension)
- Lighter weight
- Integrated in VS Code
- Install from extensions

---

#### 7. Database GUI (Optional but Helpful)

**TablePlus** (macOS/Windows - Free tier available)
- Download from [tableplus.com](https://tableplus.com/)
- Beautiful UI
- Multi-database support

**pgAdmin** (Free, all platforms)
- Download from [pgadmin.org](https://www.pgadmin.org/)
- Official PostgreSQL GUI

---

### Setup Verification Checklist

Run these commands to verify everything is installed:

```bash
# Node.js
node --version        # Should show v20+
npm --version         # Should show v10+

# Git
git --version         # Should show 2.x

# Docker
docker --version      # Should show 24.x+
docker ps             # Should show running containers

# PostgreSQL
psql --version        # Should show 16.x

# Optional: Test database connection
psql -U postgres -h localhost
# Enter password, then type: \l to list databases
```

**Expected Output:**
```
✅ node --version: v20.11.0
✅ npm --version: 10.2.4
✅ git --version: 2.42.0
✅ docker --version: 24.0.6
✅ psql --version: 16.1
```

---

## Project Setup Template

Before starting, set up your workspace:

```bash
# Create workspace directory
mkdir ~/internship-projects
cd ~/internship-projects

# Test NestJS installation
npm i -g @nestjs/cli
nest --version

# Create test project
nest new test-project
cd test-project
npm run start:dev

# Should see: Application is running on: http://localhost:3000
# Open http://localhost:3000 in browser - should see "Hello World!"
```

---

## Time Commitment

### Minimum Requirements

```csv
Program,Hours Per Week,Weeks,Total Hours
1-Month,40,4,160
2-Month,40,8,320
3-Month,40,12,480
```

**Daily Breakdown:**
- 8 hours/day
- 5 days/week
- Full-time commitment expected

**Typical Schedule:**
```
09:00-09:15 - Daily standup
09:15-12:00 - Deep work (coding/learning)
12:00-13:00 - Lunch
13:00-16:00 - Deep work
16:00-17:00 - Code review / documentation
17:00-17:30 - Mentor sync (as needed)
```

---

## Soft Skills Expectations

### Communication
- ✅ Ask questions when stuck (after trying for 30 mins)
- ✅ Provide daily progress updates
- ✅ Communicate blockers early
- ✅ Respond to messages within 2 hours (during work hours)

### Professionalism
- ✅ Be on time for meetings
- ✅ Meet deadlines (or communicate early if at risk)
- ✅ Take ownership of your work
- ✅ Accept feedback gracefully

### Learning Attitude
- ✅ Be curious and proactive
- ✅ Document what you learn
- ✅ Share knowledge with others
- ✅ Embrace challenges

---

## Pre-Program Checklist

Before your first day:

### Administrative
- [ ] Signed internship agreement
- [ ] Received company email
- [ ] Added to Slack/communication channels
- [ ] Received calendar invites for standups
- [ ] Access to GitHub organization
- [ ] Met your mentor

### Technical
- [ ] All software installed (see checklist above)
- [ ] Can run `nest new test` successfully
- [ ] Can connect to PostgreSQL
- [ ] Docker running
- [ ] VS Code configured with extensions
- [ ] Git configured with your name/email

### Knowledge
- [ ] Read through [program overview](./overview.md)
- [ ] Reviewed [tech stack details](./tech-stack.md)
- [ ] Understand program expectations
- [ ] Reviewed [evaluation criteria](./evaluation.md)

---

## Common Setup Issues

### Issue: Node version too old
```bash
# Check version
node --version  # If < v20

# macOS: Update with Homebrew
brew upgrade node

# Windows: Download latest from nodejs.org

# Linux: Use nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 20
nvm use 20
```

### Issue: Permission errors with npm
```bash
# Don't use sudo! Fix permissions instead
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'

# Add to PATH (add to ~/.bashrc or ~/.zshrc)
export PATH=~/.npm-global/bin:$PATH

# Reload shell
source ~/.bashrc  # or source ~/.zshrc
```

### Issue: PostgreSQL connection refused
```bash
# Check if running
docker ps  # Should see postgres container

# If not running
docker-compose up -d

# Test connection
psql -U postgres -h localhost -p 5432
```

### Issue: Port 3000 already in use
```bash
# Find process using port
lsof -i :3000  # macOS/Linux
netstat -ano | findstr :3000  # Windows

# Kill process or use different port
# In .env file:
PORT=3001
```

---

## First Day Agenda

Your first day will look like this:

**Morning:**
- 09:00 - Welcome meeting with team
- 09:30 - Mentor 1-on-1: Expectations and goals
- 10:00 - Development environment setup (pair with mentor)
- 11:00 - Clone and run company project locally
- 12:00 - Lunch

**Afternoon:**
- 13:00 - Code base walkthrough with mentor
- 14:00 - Review project documentation
- 15:00 - Set up first task
- 16:00 - Start learning materials (Week 1, Day 1)
- 17:00 - End-of-day sync

---

## Need Help?

**Before Program Starts:**
- Email: internship@company.com
- Slack: #internship-help

**During Program:**
- Your mentor (primary contact)
- Team Slack channel
- Weekly 1-on-1s
- Daily standups

---

## Ready to Start?

Once you've completed all prerequisites:

→ **Choose your program:**
- [1-Month Program](./1-month-program.md) - Join production project
- [2-Month Program](./2-month-program.md) - Build your own feature
- [3-Month Program](./3-month-program.md) - Create internal application

---

[← Back to Overview](./overview.md) | [Start 1-Month Program →](./1-month-program.md)
