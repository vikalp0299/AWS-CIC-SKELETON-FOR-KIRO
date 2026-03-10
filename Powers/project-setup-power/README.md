# Project Setup & Dependency Manager Power

Automatically detect, install, and configure dependencies and tools for new projects.

## Quick Start

**New Project**:
```
Create a bagel shop website using Python backend, PHP frontend, AWS SQS, and DynamoDB
```

**Add Technology**:
```
Add Redis to my project
```

**Setup Environment**:
```
Set up my development environment
```

## What It Does

- ✅ Detects required technologies from your description
- ✅ Checks what's already installed
- ✅ Installs missing languages, runtimes, and tools
- ✅ Creates dependency files (requirements.txt, package.json, etc.)
- ✅ Sets up configuration templates
- ✅ Verifies everything works

## Supported Technologies

- **Languages**: Python, Node.js, PHP, Java, Go, Rust, Ruby, .NET
- **Cloud**: AWS, Azure, GCP (CLIs and SDKs)
- **Databases**: PostgreSQL, MySQL, MongoDB, Redis, DynamoDB
- **Message Queues**: SQS, RabbitMQ, Kafka
- **Tools**: Docker, Git, and more

## Installation

This power is designed to be used within Kiro IDE. No separate installation required.

## Documentation

- [POWER.md](POWER.md) - Complete documentation
- [Getting Started Guide](steering/getting-started.md) - Step-by-step tutorial

## Example Usage

```
User: "Create a microservices app with Node.js API, Python worker, PostgreSQL, and Redis"

Kiro:
✅ Installing Node.js 18.0.0...
✅ Installing Python 3.11.0...
✅ Installing PostgreSQL client...
✅ Installing Redis client...
✅ Creating package.json...
✅ Creating requirements.txt...
✅ Creating docker-compose.yml...
✅ Setup complete!
```

## License

MIT License - See LICENSE file for details
