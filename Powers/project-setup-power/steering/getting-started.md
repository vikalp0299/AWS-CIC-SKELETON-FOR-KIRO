# Getting Started with Project Setup Power

This guide walks you through using the Project Setup & Dependency Manager power for the first time.

## Quick Start

### Scenario 1: Starting a New Project

**You say**:
```
Create a bagel shop website using Python backend, PHP frontend, AWS SQS for decoupling, and DynamoDB for storage
```

**What happens**:

1. **Analysis Phase** (5 seconds)
   - Kiro detects: Python, PHP, AWS CLI, boto3, AWS SDK for PHP
   - Checks what's already installed on your system
   - Creates installation plan

2. **Installation Phase** (2-5 minutes)
   ```
   Installing Python 3.11.0...
   Installing PHP 8.2.0...
   Installing AWS CLI 2.13.0...
   Installing boto3 1.28.0...
   Installing AWS SDK for PHP 3.0...
   ```

3. **Configuration Phase** (10 seconds)
   - Creates `requirements.txt` with Python dependencies
   - Creates `composer.json` with PHP dependencies
   - Creates `.env.example` with AWS configuration template
   - Creates `.gitignore` with common patterns

4. **Verification Phase** (5 seconds)
   ```
   ✓ Python 3.11.0 installed
   ✓ PHP 8.2.0 installed
   ✓ AWS CLI 2.13.0 installed
   ✓ boto3 1.28.0 installed
   ✓ AWS SDK for PHP 3.0 installed
   ```

5. **Summary**
   - Shows what was installed
   - Lists created files
   - Provides next steps

### Scenario 2: Adding Technology Mid-Project

**You say**:
```
Add Redis caching to my Python project
```

**What happens**:

1. **Detection Phase**
   - Kiro detects existing Python project
   - Finds `requirements.txt`
   - Checks if Redis is installed

2. **Installation Phase**
   ```
   Installing Redis client...
   Installing redis-py 4.6.0...
   ```

3. **Update Phase**
   - Adds `redis==4.6.0` to `requirements.txt`
   - Creates Redis configuration template

4. **Verification**
   ```
   ✓ Redis client installed
   ✓ redis-py 4.6.0 installed
   ✓ requirements.txt updated
   ```

### Scenario 3: Setting Up Existing Project

**You say**:
```
Set up my development environment for this project
```

**What happens**:

1. **Discovery Phase**
   - Reads `requirements.txt`, `package.json`, `composer.json`
   - Lists all required dependencies

2. **Check Phase**
   - Verifies what's already installed
   - Identifies missing dependencies

3. **Installation Phase**
   - Installs only missing dependencies

4. **Configuration Phase**
   - Sets up environment variables
   - Configures database connections

## Step-by-Step: First Project

### Step 1: Describe Your Project

Be specific about technologies:
```
Create a [PROJECT_TYPE] using:
- [BACKEND_LANGUAGE] for backend
- [FRONTEND_LANGUAGE] for frontend
- [DATABASE] for storage
- [MESSAGE_QUEUE] for async processing
- [CACHE] for caching
```

### Step 2: Review Installation Plan

Kiro will show you what will be installed:
```
Installation Plan:
- Python 3.11.0 (not installed)
- Node.js 18.0.0 (already installed ✓)
- PostgreSQL client (not installed)
- Redis client (not installed)

Proceed? (yes/no)
```

### Step 3: Wait for Installation

Installation time varies by system:
- macOS with Homebrew: 2-5 minutes
- Linux with apt: 3-7 minutes
- Windows with Chocolatey: 5-10 minutes

### Step 4: Configure Services

After installation, configure credentials:

**AWS**:
```bash
aws configure
# Enter: Access Key ID, Secret Access Key, Region
```

**Database**:
```bash
# Edit .env file
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
```

### Step 5: Install Dependencies

**Python**:
```bash
pip install -r requirements.txt
```

**Node.js**:
```bash
npm install
```

**PHP**:
```bash
composer install
```

### Step 6: Verify Setup

Test that everything works:
```bash
python --version
node --version
aws --version
psql --version
```

## Common Workflows

### Workflow 1: Full Stack Web App

```
User: "Create an e-commerce site with Node.js backend, React frontend, PostgreSQL database, and Redis cache"

Power Actions:
1. Install Node.js (if missing)
2. Install PostgreSQL client
3. Install Redis client
4. Create package.json with: express, pg, redis, react
5. Create docker-compose.yml for local services
6. Create .env.example
7. Verify installations
```

### Workflow 2: Microservices

```
User: "Build microservices with Python API, Go worker, RabbitMQ, and MongoDB"

Power Actions:
1. Install Python (if missing)
2. Install Go (if missing)
3. Install RabbitMQ client
4. Install MongoDB client
5. Create requirements.txt with: flask, pika, pymongo
6. Create go.mod with: amqp, mongo-driver
7. Create docker-compose.yml
8. Verify installations
```

### Workflow 3: Serverless App

```
User: "Create a serverless app with AWS Lambda (Python), DynamoDB, and SQS"

Power Actions:
1. Install Python (if missing)
2. Install AWS CLI
3. Install AWS SAM CLI
4. Create requirements.txt with: boto3
5. Create template.yaml for SAM
6. Create .env.example with AWS config
7. Verify installations
```

## Platform-Specific Notes

### macOS

**Package Manager**: Homebrew (auto-installed if missing)

**Common Locations**:
- Python: `/usr/local/bin/python3`
- Node.js: `/usr/local/bin/node`
- Homebrew packages: `/usr/local/Cellar/`

**Tips**:
- Use `brew update` regularly
- Use `brew upgrade` to update packages
- Use `brew doctor` to fix issues

### Linux (Ubuntu/Debian)

**Package Manager**: apt

**Common Locations**:
- Python: `/usr/bin/python3`
- Node.js: `/usr/bin/node`
- System packages: `/usr/lib/`

**Tips**:
- Run `sudo apt-get update` before installing
- Use `sudo apt-get upgrade` to update packages
- Use `which <command>` to find installation location

### Windows

**Package Manager**: Chocolatey (auto-installed if missing)

**Common Locations**:
- Python: `C:\Python311\`
- Node.js: `C:\Program Files\nodejs\`
- Chocolatey packages: `C:\ProgramData\chocolatey\`

**Tips**:
- Run PowerShell as Administrator
- Use `choco upgrade all` to update packages
- Add to PATH if command not found

## Troubleshooting

### Issue: "Command not found" after installation

**Solution**:
1. Restart your terminal
2. Check PATH: `echo $PATH` (macOS/Linux) or `echo $env:PATH` (Windows)
3. Manually add to PATH if needed

### Issue: Permission denied during installation

**Solution**:
- macOS/Linux: Use `sudo` before command
- Windows: Run PowerShell as Administrator

### Issue: Installation fails with network error

**Solution**:
1. Check internet connection
2. Try different mirror/source
3. Use VPN if behind firewall
4. Download manually and install

### Issue: Version conflict

**Solution**:
1. Use virtual environment (Python: venv, Node: nvm)
2. Specify exact version in dependency file
3. Uninstall conflicting version first

## Best Practices

### 1. Use Virtual Environments

**Python**:
```bash
python -m venv venv
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows
```

**Node.js**:
```bash
nvm install 18
nvm use 18
```

### 2. Lock Dependencies

Always commit lock files:
- `requirements.txt` (Python)
- `package-lock.json` (Node.js)
- `composer.lock` (PHP)
- `go.sum` (Go)

### 3. Use Environment Variables

Never commit secrets:
```bash
# .env (add to .gitignore)
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret
DATABASE_URL=postgresql://localhost/db
```

### 4. Document Setup

Create `README.md` with setup instructions:
```markdown
## Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Configure environment: `cp .env.example .env`
3. Run migrations: `python manage.py migrate`
4. Start server: `python manage.py runserver`
```

### 5. Use Docker for Consistency

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres:15
    environment:
      POSTGRES_DB: mydb
  redis:
    image: redis:7
```

## Next Steps

After setup is complete:

1. **Configure Services**: Set up API keys, database connections
2. **Install Dependencies**: Run package manager install commands
3. **Test Setup**: Verify everything works
4. **Start Coding**: Begin development
5. **Version Control**: Initialize git and commit

## Advanced Usage

### Custom Versions

```
Install Python 3.9 and Node.js 16
```

### Multiple Environments

```
Set up development, staging, and production environments
```

### Monorepo Setup

```
Create a monorepo with Python backend and Node.js frontend
```

### CI/CD Integration

```
Set up GitHub Actions for automated testing and deployment
```

## Getting Help

If you encounter issues:

1. **Check Logs**: Review installation output for errors
2. **Verify System**: Check OS version and available disk space
3. **Ask Kiro**: "Why did [PACKAGE] installation fail?"
4. **Manual Install**: Follow official documentation if needed

## Summary

The Project Setup Power automates:
- ✅ Language/runtime installation
- ✅ Package manager setup
- ✅ Cloud CLI installation
- ✅ Dependency file creation
- ✅ Configuration templates
- ✅ Verification and testing

Just describe your project, and let Kiro handle the setup!
