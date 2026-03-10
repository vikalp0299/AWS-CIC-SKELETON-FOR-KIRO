---
name: "project-setup"
displayName: "Project Setup & Dependency Manager"
description: "Automatically detect, install, and configure dependencies and tools for new projects across all platforms (macOS, Linux, Windows)"
keywords: ["setup", "dependencies", "install", "tools", "languages", "aws", "azure", "gcp", "python", "node", "php", "java", "go", "rust", "ruby", "dotnet", "docker", "postgres", "redis", "mongodb", "package-manager", "homebrew", "apt", "chocolatey", "pip", "npm", "composer"]
---

# Project Setup & Dependency Manager Power

Automatically detect, install, and configure dependencies and tools for new projects or when adding new technologies mid-project.

---

# Onboarding

Before using this power, Kiro will verify your system is ready for package installations.

## Step 1: Verify Package Manager

Kiro will check if your system has the appropriate package manager installed:

- **macOS**: Homebrew (`brew`)
- **Linux**: apt, yum, or pacman (depending on distribution)
- **Windows**: Chocolatey (`choco`)

If the package manager is missing, Kiro will offer to install it for you.

## Step 2: Verify Permissions

Some installations require elevated permissions:

- **macOS/Linux**: You may be prompted for your password (sudo)
- **Windows**: PowerShell should be run as Administrator

Kiro will notify you if elevated permissions are needed.

## Step 3: Ready to Use

Once the package manager is verified, you can start using the power immediately. Just describe your project requirements, and Kiro will handle the rest!

**Example**:
```
Create a web app with Python backend and PostgreSQL database
```

---

# Overview

This power provides comprehensive guidance for setting up project dependencies locally. It analyzes your project requirements and guides Kiro to:
- Detect required programming languages and runtimes
- Install necessary dependencies and tools using local package managers
- Configure cloud service SDKs (AWS, Azure, GCP)
- Set up development environment
- Create configuration files
- Verify installations

**No MCP Server Required**: This power uses local bash commands (brew, apt, choco, pip, npm, etc.) to install dependencies directly on your machine.

**Keywords**: setup, dependencies, install, tools, languages, aws, azure, gcp, python, node, php, java, go, rust

## How It Works

This power guides Kiro through the following process:

1. **Detect OS**: Check system information to determine platform (macOS, Linux, Windows)
2. **Analyze Request**: Parse user's project description to identify required technologies
3. **Check Existing**: Run platform-specific commands to verify what's installed
4. **Install Missing**: Execute appropriate installation commands based on OS
5. **Configure**: Create configuration files using `fsWrite` tool
6. **Verify**: Run version checks to confirm successful installation

**All operations use Kiro's existing tools**:
- `executeBash` - Run installation commands (bash for macOS/Linux, PowerShell for Windows)
- `fsWrite` - Create configuration files
- `grepSearch` - Check for existing project files
- `readFile` - Read dependency files

**OS Detection**: Kiro automatically detects the operating system from the system context and uses appropriate commands.

## Supported Technologies

### Programming Languages & Runtimes
- Python (pip, virtualenv, poetry)
- Node.js (npm, yarn, pnpm)
- PHP (composer)
- Java (maven, gradle)
- Go (go modules)
- Rust (cargo)
- Ruby (bundler, gem)
- .NET (dotnet CLI)

### Cloud Services
- **AWS**: AWS CLI, boto3 (Python), aws-sdk (Node.js), AWS SDK for PHP
- **Azure**: Azure CLI, azure-sdk (Python/Node.js/PHP)
- **GCP**: gcloud CLI, google-cloud SDKs

### Databases
- PostgreSQL (psql, psycopg2)
- MySQL (mysql-client, pymysql)
- MongoDB (mongosh, pymongo)
- Redis (redis-cli, redis-py)
- DynamoDB (AWS SDK)

### Message Queues
- AWS SQS (boto3, aws-sdk)
- RabbitMQ (pika, amqplib)
- Kafka (kafka-python, kafkajs)

### Development Tools
- Docker & Docker Compose
- Git
- Make
- curl, wget
- jq (JSON processor)

## Usage Examples

### Example 1: Full Stack Web App

**User Request**:
```
Create a bagel shop website using Python backend, PHP frontend, AWS SQS for decoupling, and DynamoDB for storage
```

**Power Actions**:
1. Check if Python is installed → Install if missing
2. Check if PHP is installed → Install if missing
3. Install AWS CLI
4. Install boto3 (Python AWS SDK)
5. Install AWS SDK for PHP
6. Create requirements.txt with: boto3, flask/django
7. Create composer.json with: aws/aws-sdk-php
8. Configure AWS credentials template
9. Verify all installations

### Example 2: Microservices Project

**User Request**:
```
Build a microservices app with Node.js API, Python worker service, PostgreSQL database, and Redis cache
```

**Power Actions**:
1. Check Node.js → Install if missing
2. Check Python → Install if missing
3. Install PostgreSQL client
4. Install Redis client
5. Create package.json with: express, pg, redis
6. Create requirements.txt with: psycopg2, redis
7. Create docker-compose.yml for local development
8. Verify all installations

### Example 3: Mid-Project Addition

**User Request**:
```
I need to add Elasticsearch to my existing Python project
```

**Power Actions**:
1. Detect existing Python environment
2. Install Elasticsearch client: elasticsearch-py
3. Add to requirements.txt
4. Create Elasticsearch configuration template
5. Verify installation

## Workflows

### Workflow 1: New Project Setup

```
User: "Create a [PROJECT_TYPE] using [TECHNOLOGIES]"

Power:
1. Parse technologies from request
2. Check system for existing installations
3. Install missing languages/runtimes
4. Install required SDKs and libraries
5. Create dependency files (requirements.txt, package.json, etc.)
6. Create configuration templates
7. Provide setup summary and next steps
```

### Workflow 2: Add Technology Mid-Project

```
User: "Add [TECHNOLOGY] to my project"

Power:
1. Detect existing project structure
2. Identify project language/framework
3. Install required dependencies
4. Update dependency files
5. Create configuration if needed
6. Verify installation
```

### Workflow 3: Environment Setup

```
User: "Set up my development environment for this project"

Power:
1. Read project files (package.json, requirements.txt, etc.)
2. Check installed dependencies
3. Install missing dependencies
4. Configure environment variables
5. Verify setup
```

## Installation Commands by Platform

### OS Detection

Kiro automatically detects the OS from system context. The power uses this information to select appropriate commands.

**Detection Method**:
- macOS: `platform: darwin`, `shell: bash/zsh`
- Linux: `platform: linux`, `shell: bash`
- Windows: `platform: win32`, `shell: powershell/cmd`

### Command Mapping by OS

| Task | macOS | Linux (Ubuntu/Debian) | Windows |
|------|-------|----------------------|---------|
| **Package Manager** | Homebrew | apt | Chocolatey |
| **Install Python** | `brew install python` | `sudo apt-get install python3 python3-pip` | `choco install python` |
| **Install Node.js** | `brew install node` | `curl -fsSL https://deb.nodesource.com/setup_lts.x \| sudo -E bash - && sudo apt-get install -y nodejs` | `choco install nodejs` |
| **Install PHP** | `brew install php` | `sudo apt-get install php php-cli` | `choco install php` |
| **Install AWS CLI** | `brew install awscli` | `curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && unzip awscliv2.zip && sudo ./aws/install` | `choco install awscli` |
| **Install PostgreSQL** | `brew install postgresql` | `sudo apt-get install postgresql postgresql-client` | `choco install postgresql` |
| **Install Redis** | `brew install redis` | `sudo apt-get install redis-server` | `choco install redis` |
| **Install Docker** | `brew install --cask docker` | `sudo apt-get install docker.io docker-compose` | `choco install docker-desktop` |
| **Install Git** | `brew install git` | `sudo apt-get install git` | `choco install git` |

### macOS Setup

**Check if Homebrew is installed**:
```bash
which brew
```

**Install Homebrew if missing**:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Install packages**:
```bash
brew install python node php awscli postgresql redis docker git
```

**Verify installations**:
```bash
python3 --version
node --version
php --version
aws --version
psql --version
redis-cli --version
docker --version
git --version
```

### Linux (Ubuntu/Debian) Setup

**Update package list**:
```bash
sudo apt-get update
```

**Install Python**:
```bash
sudo apt-get install -y python3 python3-pip python3-venv
```

**Install Node.js**:
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Install PHP**:
```bash
sudo apt-get install -y php php-cli php-mbstring php-xml php-curl
```

**Install AWS CLI**:
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
rm -rf aws awscliv2.zip
```

**Install PostgreSQL**:
```bash
sudo apt-get install -y postgresql postgresql-client libpq-dev
```

**Install Redis**:
```bash
sudo apt-get install -y redis-server
```

**Install Docker**:
```bash
sudo apt-get install -y docker.io docker-compose
sudo usermod -aG docker $USER
```

**Install Git**:
```bash
sudo apt-get install -y git
```

**Verify installations**:
```bash
python3 --version
node --version
php --version
aws --version
psql --version
redis-cli --version
docker --version
git --version
```

### Windows Setup

**Check if Chocolatey is installed**:
```powershell
choco --version
```

**Install Chocolatey if missing**:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

**Install packages**:
```powershell
choco install python nodejs php awscli postgresql redis docker-desktop git -y
```

**Verify installations**:
```powershell
python --version
node --version
php --version
aws --version
psql --version
redis-cli --version
docker --version
git --version
```

**Important Windows Notes**:
1. Run PowerShell as Administrator for installations
2. Restart terminal after installations to refresh PATH
3. Some tools (Docker Desktop) require system restart
4. Windows Subsystem for Linux (WSL) is recommended for better compatibility

### Cross-Platform Package Managers

| Language | Package Manager | Install Command | Works On |
|----------|----------------|-----------------|----------|
| Python | pip | `pip install <package>` | All platforms |
| Node.js | npm | `npm install <package>` | All platforms |
| PHP | composer | `composer require <package>` | All platforms |
| Ruby | gem | `gem install <package>` | All platforms |
| Go | go modules | `go get <package>` | All platforms |
| Rust | cargo | `cargo install <package>` | All platforms |
| .NET | dotnet | `dotnet add package <package>` | All platforms |

### Platform-Specific Considerations

**macOS**:
- ✅ Homebrew is the standard package manager
- ✅ Most tools install cleanly
- ⚠️ May need Xcode Command Line Tools: `xcode-select --install`
- ⚠️ Apple Silicon (M1/M2) may need Rosetta for some packages

**Linux**:
- ✅ Native environment for most development tools
- ✅ Best performance for Docker and containers
- ⚠️ Different distros use different package managers (apt, yum, pacman)
- ⚠️ May need sudo for system-level installations

**Windows**:
- ✅ Chocolatey provides consistent package management
- ✅ PowerShell is powerful and cross-platform
- ⚠️ Some tools work better in WSL (Windows Subsystem for Linux)
- ⚠️ Path separators use backslash (`\`) instead of forward slash (`/`)
- ⚠️ May need to restart terminal or system after installations
- ⚠️ Docker Desktop requires Hyper-V or WSL2 backend

## Configuration Templates

### AWS Configuration
```bash
# ~/.aws/credentials
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
region = us-east-1
```

### Python Requirements
```txt
# requirements.txt
boto3==1.28.0
flask==2.3.0
psycopg2-binary==2.9.6
redis==4.6.0
```

### Node.js Package
```json
{
  "name": "project-name",
  "version": "1.0.0",
  "dependencies": {
    "aws-sdk": "^2.1400.0",
    "express": "^4.18.0",
    "pg": "^8.11.0",
    "redis": "^4.6.0"
  }
}
```

### PHP Composer
```json
{
  "require": {
    "aws/aws-sdk-php": "^3.0",
    "php": "^8.0"
  }
}
```

## Verification Steps

After installation, the power verifies:

1. **Language/Runtime Version**:
   ```bash
   python --version
   node --version
   php --version
   ```

2. **Package Managers**:
   ```bash
   pip --version
   npm --version
   composer --version
   ```

3. **Cloud CLIs**:
   ```bash
   aws --version
   az --version
   gcloud --version
   ```

4. **Database Clients**:
   ```bash
   psql --version
   mysql --version
   redis-cli --version
   ```

## Best Practices

1. **Version Management**: Use version managers (pyenv, nvm, rbenv) for multiple versions
2. **Virtual Environments**: Create isolated environments (venv, virtualenv, conda)
3. **Dependency Locking**: Use lock files (requirements.txt, package-lock.json, composer.lock)
4. **Environment Variables**: Use .env files for configuration (never commit secrets)
5. **Docker**: Consider containerization for consistent environments

## Common Issues & Solutions

### Issue 1: Permission Denied

**Symptom**: Installation fails with permission errors

**Solution**: Use sudo (Linux/macOS) or run as Administrator (Windows)

### Issue 2: Command Not Found

**Symptom**: Installed tool not found in PATH

**Solution**: Restart terminal or add to PATH manually

### Issue 3: Version Conflicts

**Symptom**: Multiple versions causing conflicts

**Solution**: Use version managers or virtual environments

### Issue 4: Network Issues

**Symptom**: Download fails or times out

**Solution**: Check internet connection, try different mirror, or use VPN

## Integration with Kiro

### How to Use This Power

**Automatic Detection**:
```
Create a [PROJECT_DESCRIPTION]
```

Kiro will:
1. Detect required technologies
2. Activate this power
3. Install missing dependencies
4. Set up project structure
5. Provide summary

**Manual Invocation**:
```
Install dependencies for Python, AWS, and PostgreSQL
```

**Mid-Project Addition**:
```
Add Redis to my project
```

**Environment Setup**:
```
Set up my development environment
```

## Output Format

After running, the power provides:

```
✅ Project Setup Complete!

Installed:
- Python 3.11.0 ✓
- pip 23.0.1 ✓
- AWS CLI 2.13.0 ✓
- boto3 1.28.0 ✓
- PHP 8.2.0 ✓
- AWS SDK for PHP 3.0 ✓

Created Files:
- requirements.txt
- composer.json
- .env.example
- .gitignore

Next Steps:
1. Configure AWS credentials: aws configure
2. Install Python dependencies: pip install -r requirements.txt
3. Install PHP dependencies: composer install
4. Set up environment variables: cp .env.example .env
5. Start development!

Documentation:
- AWS SDK for Python: https://boto3.amazonaws.com/v1/documentation/api/latest/index.html
- AWS SDK for PHP: https://docs.aws.amazon.com/sdk-for-php/
```

## Technology Detection Patterns

The power uses these patterns to detect required technologies:

| User Mentions | Installs |
|---------------|----------|
| "Python backend" | Python, pip, virtualenv |
| "Node.js API" | Node.js, npm |
| "PHP frontend" | PHP, composer |
| "AWS SQS" | AWS CLI, boto3/aws-sdk |
| "DynamoDB" | AWS CLI, boto3/aws-sdk |
| "PostgreSQL" | PostgreSQL client, psycopg2/pg |
| "Redis cache" | Redis client, redis-py/redis |
| "MongoDB" | MongoDB client, pymongo/mongodb |
| "Docker" | Docker, docker-compose |
| "Elasticsearch" | Elasticsearch client |
| "RabbitMQ" | RabbitMQ client, pika/amqplib |
| "Kafka" | Kafka client, kafka-python/kafkajs |

## Limitations

1. **System Permissions**: May require sudo/admin access
2. **Network Dependency**: Requires internet connection
3. **Platform Specific**: Commands vary by OS
4. **Version Selection**: Installs latest stable by default
5. **Manual Configuration**: Some services require manual setup (API keys, credentials)

## Future Enhancements

- [ ] Support for more languages (Kotlin, Swift, Scala)
- [ ] Automatic version selection based on project requirements
- [ ] Dependency conflict resolution
- [ ] Automatic cloud service provisioning
- [ ] IDE/Editor setup (VS Code extensions, etc.)
- [ ] Database schema initialization
- [ ] CI/CD pipeline setup

## Related Powers

- **AWS Power**: Manage AWS resources
- **Docker Power**: Container management
- **Database Power**: Database operations
- **Deployment Power**: Deploy applications

## Support

For issues or feature requests, please refer to Kiro documentation or create a support ticket.

---

**Version**: 1.0.0  
**Last Updated**: 2026-03-09  
**Maintainer**: Kiro Team
