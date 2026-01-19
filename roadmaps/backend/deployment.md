---
sidebar_position: 7
---

# Deployment & DevOps

Learn to deploy, monitor, and maintain production backend systems.

## Deployment Fundamentals

### Environment Types

1. **Development**: Local machine, frequent changes
2. **Staging**: Pre-production testing environment
3. **Production**: Live application serving real users

### Environment Variables

Never hardcode sensitive data. Use environment variables.

```javascript
// .env file (NEVER commit to git)
DATABASE_URL=postgresql://user:pass@localhost:5432/mydb
JWT_SECRET=your-secret-key-here
API_KEY=sk_live_1234567890
NODE_ENV=production

// Load in your app
require('dotenv').config();

const dbUrl = process.env.DATABASE_URL;
const jwtSecret = process.env.JWT_SECRET;
```

---

## Containerization with Docker

Package your application with all dependencies into a container.

### Dockerfile Example

```dockerfile
# Use official Node.js image
FROM node:20-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Start application
CMD ["node", "server.js"]
```

### docker-compose.yml

Run multiple containers together.

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://postgres:password@db:5432/mydb
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    volumes:
      - postgres-data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres-data:
```

### Docker Commands

```bash
# Build image
docker build -t myapp:latest .

# Run container
docker run -p 3000:3000 --env-file .env myapp:latest

# Run with docker-compose
docker-compose up -d

# View logs
docker-compose logs -f app

# Stop containers
docker-compose down
```

---

## Cloud Platforms

### 1. AWS (Amazon Web Services)

**Popular Services:**
- **EC2**: Virtual servers
- **ECS/EKS**: Container orchestration
- **Lambda**: Serverless functions
- **RDS**: Managed databases
- **S3**: Object storage
- **CloudFront**: CDN
- **Route 53**: DNS
- **Elastic Beanstalk**: Platform as a Service

**Deployment Example (Elastic Beanstalk):**
```bash
# Install EB CLI
pip install awsebcli

# Initialize
eb init

# Create environment
eb create production

# Deploy
eb deploy

# View logs
eb logs
```

---

### 2. Google Cloud Platform (GCP)

**Popular Services:**
- **Compute Engine**: Virtual machines
- **Cloud Run**: Serverless containers
- **GKE**: Kubernetes engine
- **Cloud SQL**: Managed databases
- **Cloud Storage**: Object storage
- **Cloud CDN**: Content delivery

---

### 3. Microsoft Azure

**Popular Services:**
- **Virtual Machines**: Compute instances
- **App Service**: PaaS for web apps
- **AKS**: Kubernetes service
- **Azure SQL**: Managed databases
- **Blob Storage**: Object storage

---

### 4. Platform as a Service (PaaS)

Simple deployment without managing infrastructure.

**Vercel:**
```bash
npm install -g vercel
vercel --prod
```

**Render:**
- Connect GitHub repo
- Auto-deploy on push
- Free tier available

**Railway:**
```bash
npm install -g @railway/cli
railway login
railway init
railway up
```

**Fly.io:**
```bash
fly launch
fly deploy
```

**DigitalOcean App Platform:**
- Connect repo
- Configure build/run commands
- Deploy

---

## Continuous Integration/Continuous Deployment (CI/CD)

Automate testing and deployment.

### GitHub Actions

**.github/workflows/deploy.yml:**
```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Run linter
        run: npm run lint

  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to production
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
        run: |
          npm ci
          npm run build
          # Deploy commands here
```

---

### GitLab CI/CD

**.gitlab-ci.yml:**
```yaml
stages:
  - test
  - build
  - deploy

variables:
  NODE_VERSION: "20"

test:
  stage: test
  image: node:$NODE_VERSION
  script:
    - npm ci
    - npm test
    - npm run lint

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t myapp:$CI_COMMIT_SHA .
    - docker push myapp:$CI_COMMIT_SHA

deploy:
  stage: deploy
  only:
    - main
  script:
    - kubectl set image deployment/myapp myapp=myapp:$CI_COMMIT_SHA
```

---

## Database Migrations

Manage database schema changes.

### Prisma Migrations

```bash
# Create migration
npx prisma migrate dev --name add_users_table

# Apply to production
npx prisma migrate deploy
```

### Knex Migrations

```bash
# Create migration
npx knex migrate:make add_users_table

# Run migrations
npx knex migrate:latest

# Rollback
npx knex migrate:rollback
```

### TypeORM Migrations

```bash
# Generate migration
npm run typeorm migration:generate -- -n AddUsersTable

# Run migrations
npm run typeorm migration:run

# Revert
npm run typeorm migration:revert
```

---

## Monitoring & Logging

### Application Monitoring

**Tools:**
- **Prometheus + Grafana**: Open-source monitoring
- **Datadog**: All-in-one monitoring
- **New Relic**: APM (Application Performance Monitoring)
- **Sentry**: Error tracking
- **Logtail/Papertrail**: Log management

### Health Checks

```javascript
// Simple health check endpoint
app.get('/health', async (req, res) => {
  try {
    // Check database connection
    await db.raw('SELECT 1');
    
    // Check Redis connection
    await redis.ping();
    
    res.json({
      status: 'healthy',
      timestamp: new Date().toISOString(),
      uptime: process.uptime()
    });
  } catch (error) {
    res.status(503).json({
      status: 'unhealthy',
      error: error.message
    });
  }
});
```

### Structured Logging

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

// Usage
logger.info('User logged in', { userId: user.id, email: user.email });
logger.error('Database error', { error: err.message, stack: err.stack });
```

---

## Process Management

Keep your Node.js app running.

### PM2

```bash
# Install globally
npm install -g pm2

# Start app
pm2 start server.js --name myapp

# Start with environment
pm2 start server.js --env production

# List processes
pm2 list

# Monitor
pm2 monit

# Logs
pm2 logs myapp

# Restart
pm2 restart myapp

# Stop
pm2 stop myapp

# Startup script (run on boot)
pm2 startup
pm2 save
```

**ecosystem.config.js:**
```javascript
module.exports = {
  apps: [{
    name: 'myapp',
    script: './server.js',
    instances: 'max', // Use all CPU cores
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'development',
    },
    env_production: {
      NODE_ENV: 'production',
    }
  }]
};

// Start with config
// pm2 start ecosystem.config.js --env production
```

---

## Reverse Proxy & Web Server

### NGINX

Route traffic, handle SSL, serve static files.

**/etc/nginx/sites-available/myapp:**
```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    # Redirect to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name example.com www.example.com;

    # SSL certificates
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # Security headers
    add_header Strict-Transport-Security "max-age=31536000" always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;

    # Proxy to Node.js app
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }

    # Serve static files directly
    location /static/ {
        alias /var/www/myapp/static/;
        expires 30d;
        add_header Cache-Control "public, immutable";
    }
}
```

### Enable Configuration

```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/

# Test configuration
sudo nginx -t

# Reload NGINX
sudo systemctl reload nginx
```

---

## SSL/TLS Certificates

### Let's Encrypt (Free SSL)

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Get certificate
sudo certbot --nginx -d example.com -d www.example.com

# Auto-renewal (runs automatically)
sudo certbot renew --dry-run
```

---

## Database Backups

### PostgreSQL Backup

```bash
# Backup
pg_dump -U postgres -h localhost mydb > backup.sql

# Restore
psql -U postgres -h localhost mydb < backup.sql

# Automated daily backups (cron)
# Add to crontab: crontab -e
0 2 * * * pg_dump -U postgres mydb > /backups/mydb_$(date +\%Y\%m\%d).sql
```

### MongoDB Backup

```bash
# Backup
mongodump --uri="mongodb://localhost:27017/mydb" --out=/backups/

# Restore
mongorestore --uri="mongodb://localhost:27017/mydb" /backups/mydb/
```

---

## Security Best Practices

### 1. Firewall Configuration

```bash
# Ubuntu/Debian with UFW
sudo ufw enable
sudo ufw allow 22/tcp    # SSH
sudo ufw allow 80/tcp    # HTTP
sudo ufw allow 443/tcp   # HTTPS
sudo ufw status
```

### 2. SSH Security

```bash
# Disable password authentication
# Edit /etc/ssh/sshd_config
PasswordAuthentication no
PubkeyAuthentication yes

# Restart SSH
sudo systemctl restart sshd
```

### 3. Keep Software Updated

```bash
# Ubuntu/Debian
sudo apt update && sudo apt upgrade -y

# Enable automatic security updates
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

### 4. Secrets Management

**Tools:**
- **AWS Secrets Manager**
- **Azure Key Vault**
- **HashiCorp Vault**
- **Doppler**
- **Infisical**

---

## Scaling Strategies

### 1. Vertical Scaling
Upgrade server resources (CPU, RAM)

### 2. Horizontal Scaling
Add more servers behind a load balancer

### 3. Auto-Scaling
Automatically add/remove servers based on traffic

**AWS Auto Scaling Example:**
- Set min/max instances
- Scale based on CPU usage, request count
- Automatically adds instances when traffic spikes

---

## Kubernetes (Advanced)

Container orchestration for large-scale deployments.

**deployment.yaml:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 3000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: myapp-secrets
              key: database-url
---
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 3000
  selector:
    app: myapp
```

```bash
# Deploy
kubectl apply -f deployment.yaml

# Scale
kubectl scale deployment myapp --replicas=5

# View pods
kubectl get pods

# View logs
kubectl logs -f deployment/myapp
```

---

## Learning Path

1. **Start simple**
   - Deploy to PaaS (Render, Railway, Vercel)
   - Understand environment variables
   - Set up basic monitoring

2. **Learn Docker**
   - Containerize your application
   - Use docker-compose for local development
   - Push images to Docker Hub

3. **Set up CI/CD**
   - GitHub Actions for automated testing
   - Deploy on every push to main
   - Run tests before deployment

4. **Master a cloud provider**
   - Learn AWS/GCP/Azure basics
   - Deploy to VPS or container service
   - Set up databases and storage

5. **Production readiness**
   - SSL certificates
   - NGINX reverse proxy
   - Database backups
   - Monitoring and alerts

6. **Advanced topics**
   - Kubernetes
   - Multi-region deployment
   - Auto-scaling
   - Infrastructure as Code (Terraform)

## Practice Projects

1. **Deploy Simple API** - Use Render/Railway for quick deployment
2. **Dockerize Application** - Create Dockerfile and docker-compose
3. **CI/CD Pipeline** - Set up GitHub Actions for automated deployment
4. **VPS Deployment** - Deploy to DigitalOcean/AWS EC2 with NGINX
5. **Full Production Stack** - Load balancer, auto-scaling, monitoring

## Next Steps

- **Projects**: Build complete full-stack applications
- **Advanced DevOps**: Kubernetes, Terraform, microservices
- **Security**: Penetration testing, security audits
