# Postman MCP Integration Guide

A comprehensive guide for integrating Postman's Model Context Protocol (MCP) with Kiro IDE to automate API testing for localhost applications.

---

## Overview

Postman MCP allows you to programmatically manage Postman collections, environments, and run automated API tests directly from Kiro IDE. Since Postman's cloud service cannot directly access `localhost`, this guide shows you how to use a tunnel service to expose your local API.

**Key Benefits**:
- Automate API testing from Kiro IDE
- Manage collections and environments programmatically
- Run tests as part of your development workflow
- Let Kiro handle best practices automatically

---

## Prerequisites

### Required Software
- **Kiro IDE** with Postman MCP power installed
- **Node.js** (v14+) for tunnel service
- **Postman Account** (free or paid) at [postman.com](https://postman.com)
- **Local API Server** running on any port (e.g., 3000, 5000, 8080)

### Required Credentials
- Postman API Key (obtain from Settings → API Keys at postman.com)

---

## Initial Setup (Required Once)

### Get Your Postman API Key

1. Log into [postman.com](https://postman.com)
2. Navigate to **Settings** → **API Keys**
3. Click **Generate API Key**
4. Copy the generated key

### Add API Key to Kiro MCP Configuration

**File Location**: `~/.kiro/settings/mcp.json`

**Configuration**:
```json
{
  "mcpServers": {
    "postman": {
      "url": "https://mcp.postman.com/minimal",
      "headers": {
        "Authorization": "Bearer YOUR_POSTMAN_API_KEY_HERE"
      }
    }
  }
}
```

**Replace** `YOUR_POSTMAN_API_KEY_HERE` with your actual API key.

**Note**: The `/minimal` endpoint provides 40 essential tools. For advanced features (112 tools), use `/full` instead.

### Verify Configuration

In Kiro chat, ask:
```
Can you check if the Postman power is configured correctly?
```

Kiro will activate the power and confirm it's working.

---

## Choose Your Approach

After completing the initial setup above, choose one of the following approaches:

---

## Part A: Manual Approach (Learn the Integration)

This approach walks you through each step manually to understand how Postman MCP integration works.

### Step 1: Get Your User Information

Ask Kiro:
```
Get my Postman user information
```

Save your **User ID** and **Team ID** for later use.

### Step 2: Create or Select a Workspace

**List existing workspaces**:
```
List my Postman workspaces
```

**Or create a new workspace**:
```
Create a new Postman workspace called "My API Testing" as a personal workspace
```

Save the **Workspace ID**.

### Step 3: Create a Collection

Ask Kiro:
```
Create a Postman collection called "My API Tests" in workspace [YOUR_WORKSPACE_ID]
```

Save both the **collection.id** and **collection.uid**.

### Step 4: Create an Environment

Ask Kiro:
```
Create a Postman environment called "Local Development" in workspace [YOUR_WORKSPACE_ID] with variables:
- base_url: http://localhost:3000
- auth_token: (empty, secret type)
```

Save the **environment.id** and **environment.uid**.

### Step 5: Add Requests to Collection

**Example: Add a GET request**:
```
Add a GET request to collection [YOUR_COLLECTION_ID]:
- Name: Get Users
- URL: {{base_url}}/api/users
- Headers: Content-Type: application/json
- Tests: Check status is 200 and response is an array
```

**Example: Add a POST request**:
```
Add a POST request to collection [YOUR_COLLECTION_ID]:
- Name: Create User
- URL: {{base_url}}/api/users
- Body: {"name": "John Doe", "email": "john@example.com"}
- Tests: Check status is 201 and response has id property
```

**Example: Add an authenticated request**:
```
Add a GET request to collection [YOUR_COLLECTION_ID]:
- Name: Get Profile
- URL: {{base_url}}/api/profile
- Headers: Authorization: Bearer {{auth_token}}
- Tests: Check status is 200
```

### Step 6: Set Up Tunnel for Localhost

**Install localtunnel** (if not already installed):
```bash
npm install -g localtunnel
```

**Start your API server**:
```bash
npm start  # or python app.py, dotnet run, etc.
```

**Start the tunnel**:
```bash
lt --port 3000  # Replace 3000 with your API port
```

**Expected output**:
```
your url is: https://smooth-bags-crash.loca.lt
```

**Important**: Keep this terminal window open. The URL changes every time you restart.

### Step 7: Update Environment with Tunnel URL

Ask Kiro:
```
Update Postman environment [YOUR_ENVIRONMENT_ID] to set base_url to https://smooth-bags-crash.loca.lt
```

Replace the URL with your actual tunnel URL from Step 6.

### Step 8: Run the Collection

Ask Kiro:
```
Run Postman collection [YOUR_COLLECTION_UID] with environment [YOUR_ENVIRONMENT_ID]
```

**Important**: Use the collection **UID** (not the regular ID) for running collections.

### Step 9: Review Results

Kiro will show you:
- Total tests passed/failed
- Success rate
- Individual request results
- Error messages for failed tests
- Suggestions for fixes

---

## Part B: Automated Approach (Quick Start)

This approach lets Kiro handle everything automatically after the initial setup.

### Prerequisites
- ✅ Completed initial setup (API key configuration)
- ✅ API server running locally
- ✅ Know your API port number

### Single Command Setup

Ask Kiro:
```
Create a Postman collection called "My API Tests" and test my API at localhost:3000:
1. Create a workspace if needed
2. Create a collection with requests for my API endpoints
3. Create an environment with proper variables
4. Set up a localtunnel
5. Run the collection and show me the results
6. Apply best practices for test assertions and dynamic data
```

**That's it!** Kiro will:
- ✅ Create or select a workspace
- ✅ Create a collection with your API name
- ✅ Create an environment with base_url and auth_token variables
- ✅ Start a localtunnel for your localhost API
- ✅ Add requests for your API endpoints (if you provide them)
- ✅ Add proper test scripts with assertions
- ✅ Add dynamic data generation to avoid conflicts
- ✅ Run the collection and provide detailed results
- ✅ Suggest fixes for any failing tests

### For Subsequent Runs

After the initial setup, simply ask:
```
Run my Postman tests again with a fresh tunnel
```

Or if you want to add more requests:
```
Add a POST request to my collection for creating a blog post
```

Or if you want to update tests:
```
Update my Postman collection to use dynamic email addresses in the registration test
```

---

## Common Issues and Solutions

### Issue 1: 401 Unauthorized Errors

**Symptoms**: Authenticated endpoints return 401 even with valid credentials

**Cause**: Token not being saved or sent correctly

**Solution**: Ask Kiro to update the test scripts to save tokens from login responses or cookies.

### Issue 2: Tunnel URL Changes

**Symptoms**: Tests fail after restarting tunnel

**Cause**: Localtunnel generates new URL on each start

**Solution**: Ask Kiro to update the environment with the new tunnel URL, or use ngrok with a reserved domain (paid feature).

### Issue 3: CORS Errors

**Symptoms**: Requests fail with CORS policy errors

**Cause**: API not configured to accept requests from tunnel domain

**Solution**: Configure CORS in your API to accept tunnel URLs:

```javascript
// Example for Express.js
app.use(cors({
  origin: [
    'http://localhost:3000',
    'https://*.loca.lt',  // Allow all localtunnel URLs
    'https://*.ngrok.io'  // Allow all ngrok URLs
  ],
  credentials: true
}));
```

### Issue 4: Test Data Conflicts

**Symptoms**: Tests fail because data already exists (e.g., "User already exists")

**Cause**: Static test data used in multiple runs

**Solution**: Ask Kiro to add dynamic data generation to your collection. Kiro will automatically add timestamp-based or UUID-based unique identifiers.

### Issue 5: Request Timeout

**Symptoms**: Requests fail with timeout errors

**Causes**:
1. API server not responding
2. Tunnel connection slow
3. Request taking too long

**Solutions**:
- Verify API server is running
- Check tunnel connection
- Ask Kiro to increase the timeout in collection run options

---

## Quick Reference

### Manual Commands

**Start Local API**:
```bash
npm start  # or python app.py, dotnet run, etc.
```

**Start Tunnel**:
```bash
lt --port YOUR_PORT
```

### Kiro Commands

**Full Automated Setup**:
```
Create a Postman collection called "My API Tests" and test my API at localhost:3000
```

**Run Existing Collection**:
```
Run Postman collection [YOUR_COLLECTION_UID] with environment [YOUR_ENVIRONMENT_ID]
```

**Update Environment**:
```
Update Postman environment [YOUR_ENVIRONMENT_ID] to set base_url to [YOUR_TUNNEL_URL]
```

**Add Request**:
```
Add a [METHOD] request to collection [YOUR_COLLECTION_ID] for [ENDPOINT_DESCRIPTION]
```

**Rerun with Fresh Tunnel**:
```
Run my Postman tests again with a fresh tunnel
```

---

## Configuration File (Optional)

You can create a `.postman.json` file in your project root to store IDs for easy reference:

```json
{
  "collectionId": "YOUR_COLLECTION_ID",
  "collectionUid": "YOUR_COLLECTION_UID",
  "workspaceId": "YOUR_WORKSPACE_ID",
  "environmentId": "YOUR_ENVIRONMENT_ID",
  "environmentUid": "YOUR_ENVIRONMENT_UID",
  "baseUrl": "http://localhost:3000",
  "publicTunnelUrl": "",
  "description": "API Testing Configuration"
}
```

Kiro can read this file to automatically use the correct IDs.

---

## Alternative Tunnel Services

While this guide uses **localtunnel** (free and simple), you can also use:

### ngrok (Recommended for Production)
```bash
# Install
brew install ngrok  # macOS
# or download from ngrok.com

# Start tunnel
ngrok http 3000

# With reserved domain (paid)
ngrok http 3000 --domain=your-reserved-domain.ngrok.io
```

**Benefits**: More stable, reserved domains available, better performance

### Cloudflare Tunnel (Enterprise)
```bash
# Install
brew install cloudflare/cloudflare/cloudflared

# Start tunnel
cloudflared tunnel --url http://localhost:3000
```

**Benefits**: Enterprise-grade, DDoS protection, no bandwidth limits

### serveo (SSH-based)
```bash
ssh -R 80:localhost:3000 serveo.net
```

**Benefits**: No installation required, SSH-based

---

## Additional Resources

- [Postman Learning Center](https://learning.postman.com)
- [Postman API Documentation](https://www.postman.com/postman/workspace/postman-public-workspace/documentation/12959542-c8142d51-e97c-46b6-bd77-52bb66712c9a)
- [Localtunnel Documentation](https://theboroer.github.io/localtunnel-www/)
- [ngrok Documentation](https://ngrok.com/docs)

---

## Summary

**Part A (Manual)**: Learn how Postman MCP integration works by going through each step manually. Great for understanding the process.

**Part B (Automated)**: Let Kiro handle everything automatically with a single command. Great for quick setup and ongoing use.

**Recommendation**: Try Part A once to understand the integration, then use Part B for all future projects.

---

## License

This guide is provided as-is for educational purposes.
