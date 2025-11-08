# Pipedrive MCP Server (Extended) - AI Agent Installation Guide

This guide provides comprehensive installation instructions for AI agents installing the Pipedrive MCP server (Extended) in OI OS (Brain Trust 4) environments, as well as end-user setup instructions.

## Table of Contents

1. [AI Agent Quick Installation](#ai-agent-quick-installation)
2. [Prerequisites](#prerequisites)
3. [Full Installation Steps](#full-installation-steps)
4. [Configuring Authentication](#configuring-authentication)
5. [Connecting to OI OS](#connecting-to-oi-os)
6. [Creating Intent Mappings](#creating-intent-mappings)
7. [Creating Parameter Rules](#creating-parameter-rules)
8. [End User Setup](#end-user-setup)
9. [Verification & Testing](#verification--testing)
10. [Troubleshooting](#troubleshooting)
11. [Available Tools Reference](#available-tools-reference)

---

## AI Agent Quick Installation

**⚠️ For AI Agents: Use Direct Calls for Reliability**

AI agents should prefer **direct `brain-trust4 call` commands** over natural language queries for maximum reliability. Natural language commands can timeout or have parameter extraction issues. Direct calls bypass intent mapping and parameter extraction, providing consistent results.

**Example:**
```bash
# ✅ RECOMMENDED for AI agents: Direct call
./brain-trust4 call pipedrive-mcp-server-extended get-deals '{}'

# ⚠️ FALLBACK: Natural language (may timeout or fail parameter extraction)
./oi "pipedrive get deals"
```

For AI agents using OI OS, execute the following commands:

```bash
# 1. Install the server
./oi install https://github.com/OI-OS/pipedrive-mcp-server-extended.git

# 2. Build the project
cd MCP-servers/pipedrive-mcp-server-extended
npm install
npm run build
cd ../../

# 3. Configure authentication in project root .env file
# Add to .env:
# PIPEDRIVE_API_TOKEN=your_api_token_here
# PIPEDRIVE_DOMAIN=your-company.pipedrive.com
# See "Configuring Authentication" section for detailed instructions

# 4. Connect the server to OI OS
# NOTE: brain-trust4 automatically loads .env file from project root
export $(grep -E "^PIPEDRIVE" .env | xargs)
./brain-trust4 connect pipedrive-mcp-server-extended node -- "$(pwd)/MCP-servers/pipedrive-mcp-server-extended/build/index.js"

# 5. Create intent mappings and parameter rules (single optimized transaction)
sqlite3 brain-trust4.db << 'SQL'
BEGIN TRANSACTION;

-- Intent mappings for Pipedrive MCP server (all 19 tools)
INSERT OR REPLACE INTO intent_mappings (keyword, server_name, tool_name, priority) VALUES 
-- Read operations
('pipedrive get users', 'pipedrive-mcp-server-extended', 'get-users', 10),
('pipedrive users', 'pipedrive-mcp-server-extended', 'get-users', 10),
('pipedrive list users', 'pipedrive-mcp-server-extended', 'get-users', 10),
('pipedrive get deals', 'pipedrive-mcp-server-extended', 'get-deals', 10),
('pipedrive deals', 'pipedrive-mcp-server-extended', 'get-deals', 10),
('pipedrive list deals', 'pipedrive-mcp-server-extended', 'get-deals', 10),
('pipedrive get deal', 'pipedrive-mcp-server-extended', 'get-deal', 10),
('pipedrive deal', 'pipedrive-mcp-server-extended', 'get-deal', 10),
('pipedrive get deal notes', 'pipedrive-mcp-server-extended', 'get-deal-notes', 10),
('pipedrive deal notes', 'pipedrive-mcp-server-extended', 'get-deal-notes', 10),
('pipedrive search deals', 'pipedrive-mcp-server-extended', 'search-deals', 10),
('pipedrive get persons', 'pipedrive-mcp-server-extended', 'get-persons', 10),
('pipedrive persons', 'pipedrive-mcp-server-extended', 'get-persons', 10),
('pipedrive contacts', 'pipedrive-mcp-server-extended', 'get-persons', 10),
('pipedrive list persons', 'pipedrive-mcp-server-extended', 'get-persons', 10),
('pipedrive get person', 'pipedrive-mcp-server-extended', 'get-person', 10),
('pipedrive person', 'pipedrive-mcp-server-extended', 'get-person', 10),
('pipedrive search persons', 'pipedrive-mcp-server-extended', 'search-persons', 10),
('pipedrive search contacts', 'pipedrive-mcp-server-extended', 'search-persons', 10),
('pipedrive get organizations', 'pipedrive-mcp-server-extended', 'get-organizations', 10),
('pipedrive organizations', 'pipedrive-mcp-server-extended', 'get-organizations', 10),
('pipedrive companies', 'pipedrive-mcp-server-extended', 'get-organizations', 10),
('pipedrive list organizations', 'pipedrive-mcp-server-extended', 'get-organizations', 10),
('pipedrive get organization', 'pipedrive-mcp-server-extended', 'get-organization', 10),
('pipedrive organization', 'pipedrive-mcp-server-extended', 'get-organization', 10),
('pipedrive search organizations', 'pipedrive-mcp-server-extended', 'search-organizations', 10),
('pipedrive get pipelines', 'pipedrive-mcp-server-extended', 'get-pipelines', 10),
('pipedrive pipelines', 'pipedrive-mcp-server-extended', 'get-pipelines', 10),
('pipedrive list pipelines', 'pipedrive-mcp-server-extended', 'get-pipelines', 10),
('pipedrive get pipeline', 'pipedrive-mcp-server-extended', 'get-pipeline', 10),
('pipedrive pipeline', 'pipedrive-mcp-server-extended', 'get-pipeline', 10),
('pipedrive get stages', 'pipedrive-mcp-server-extended', 'get-stages', 10),
('pipedrive stages', 'pipedrive-mcp-server-extended', 'get-stages', 10),
('pipedrive search leads', 'pipedrive-mcp-server-extended', 'search-leads', 10),
('pipedrive search all', 'pipedrive-mcp-server-extended', 'search-all', 10),
-- Write operations
('pipedrive create deal', 'pipedrive-mcp-server-extended', 'create-deal', 10),
('pipedrive add deal', 'pipedrive-mcp-server-extended', 'create-deal', 10),
('pipedrive new deal', 'pipedrive-mcp-server-extended', 'create-deal', 10),
('pipedrive create person', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive add person', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive new person', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive create contact', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive add contact', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive create organization', 'pipedrive-mcp-server-extended', 'create-organization', 10),
('pipedrive add organization', 'pipedrive-mcp-server-extended', 'create-organization', 10),
('pipedrive new organization', 'pipedrive-mcp-server-extended', 'create-organization', 10),
('pipedrive create company', 'pipedrive-mcp-server-extended', 'create-organization', 10),
('pipedrive add company', 'pipedrive-mcp-server-extended', 'create-organization', 10);

-- Parameter rules for Pipedrive MCP server
-- Read operations with no required fields
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('pipedrive-mcp-server-extended', 'get-users', 'pipedrive-mcp-server-extended::get-users', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-deals', 'pipedrive-mcp-server-extended::get-deals', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-persons', 'pipedrive-mcp-server-extended::get-persons', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-organizations', 'pipedrive-mcp-server-extended::get-organizations', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-pipelines', 'pipedrive-mcp-server-extended::get-pipelines', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-stages', 'pipedrive-mcp-server-extended::get-stages', '[]', '{}', '[]');

-- Read operations with required ID fields
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('pipedrive-mcp-server-extended', 'get-deal', 'pipedrive-mcp-server-extended::get-deal', '["dealId"]',
 '{"dealId": {"FromQuery": "pipedrive-mcp-server-extended::get-deal.dealId"}}', '[]'),
('pipedrive-mcp-server-extended', 'get-deal-notes', 'pipedrive-mcp-server-extended::get-deal-notes', '["dealId"]',
 '{"dealId": {"FromQuery": "pipedrive-mcp-server-extended::get-deal-notes.dealId"}}', '[]'),
('pipedrive-mcp-server-extended', 'get-person', 'pipedrive-mcp-server-extended::get-person', '["personId"]',
 '{"personId": {"FromQuery": "pipedrive-mcp-server-extended::get-person.personId"}}', '[]'),
('pipedrive-mcp-server-extended', 'get-organization', 'pipedrive-mcp-server-extended::get-organization', '["organizationId"]',
 '{"organizationId": {"FromQuery": "pipedrive-mcp-server-extended::get-organization.organizationId"}}', '[]'),
('pipedrive-mcp-server-extended', 'get-pipeline', 'pipedrive-mcp-server-extended::get-pipeline', '["pipelineId"]',
 '{"pipelineId": {"FromQuery": "pipedrive-mcp-server-extended::get-pipeline.pipelineId"}}', '[]');

-- Search operations with required term
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('pipedrive-mcp-server-extended', 'search-deals', 'pipedrive-mcp-server-extended::search-deals', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-deals.term"}}', '[]'),
('pipedrive-mcp-server-extended', 'search-persons', 'pipedrive-mcp-server-extended::search-persons', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-persons.term"}}', '[]'),
('pipedrive-mcp-server-extended', 'search-organizations', 'pipedrive-mcp-server-extended::search-organizations', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-organizations.term"}}', '[]'),
('pipedrive-mcp-server-extended', 'search-leads', 'pipedrive-mcp-server-extended::search-leads', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-leads.term"}}', '[]'),
('pipedrive-mcp-server-extended', 'search-all', 'pipedrive-mcp-server-extended::search-all', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-all.term"}}', '[]');

-- Create operations with required fields
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('pipedrive-mcp-server-extended', 'create-deal', 'pipedrive-mcp-server-extended::create-deal', '["title", "stageId", "ownerId"]',
 '{"title": {"FromQuery": "pipedrive-mcp-server-extended::create-deal.title"}, "stageId": {"FromQuery": "pipedrive-mcp-server-extended::create-deal.stageId"}, "ownerId": {"FromQuery": "pipedrive-mcp-server-extended::create-deal.ownerId"}}', '[]'),
('pipedrive-mcp-server-extended', 'create-person', 'pipedrive-mcp-server-extended::create-person', '["name"]',
 '{"name": {"FromQuery": "pipedrive-mcp-server-extended::create-person.name"}}', '[]'),
('pipedrive-mcp-server-extended', 'create-organization', 'pipedrive-mcp-server-extended::create-organization', '["name"]',
 '{"name": {"FromQuery": "pipedrive-mcp-server-extended::create-organization.name"}}', '[]');

COMMIT;
SQL

# 6. Add parameter extractors to TOML (already in parameter_extractors.toml.default)
# The extractors for all Pipedrive tools are already configured

# 7. Verify installation
./oi list | grep pipedrive
./brain-trust4 call pipedrive-mcp-server-extended get-users '{}'
./oi "pipedrive get deals"
```

---

## Prerequisites

- Node.js 18 or higher
- npm (Node Package Manager)
- Pipedrive account with API access
- OI OS (Brain Trust 4) installed and configured

---

## Full Installation Steps

### Step 1: Install the Server

```bash
./oi install https://github.com/OI-OS/pipedrive-mcp-server-extended.git
```

This will clone the repository to `MCP-servers/pipedrive-mcp-server-extended/`.

### Step 2: Build the Project

```bash
cd MCP-servers/pipedrive-mcp-server-extended
npm install
npm run build
cd ../../
```

The build process compiles TypeScript to JavaScript in the `build/` directory.

### Step 3: Configure Authentication

See the [Configuring Authentication](#configuring-authentication) section for detailed instructions on obtaining and configuring your Pipedrive API credentials.

### Step 4: Connect to OI OS

```bash
export $(grep -E "^PIPEDRIVE" .env | xargs)
./brain-trust4 connect pipedrive-mcp-server-extended node -- "$(pwd)/MCP-servers/pipedrive-mcp-server-extended/build/index.js"
```

### Step 5: Create Intent Mappings and Parameter Rules

See the [Creating Intent Mappings](#creating-intent-mappings) and [Creating Parameter Rules](#creating-parameter-rules) sections for the complete SQL statements.

---

## Configuring Authentication

Pipedrive MCP server uses API token authentication. You need to obtain your API token and domain from your Pipedrive account.

### Getting Pipedrive API Credentials

1. **Log in to Pipedrive**
   - Go to https://www.pipedrive.com
   - Log in to your account

2. **Navigate to Settings**
   - Click on your profile icon (top right)
   - Select "Personal preferences" or "Settings"
   - Go to "API" section

3. **Generate API Token**
   - In the API section, find "Your personal API token"
   - Click "Generate" or "Create token" if you don't have one
   - **Important:** Copy the token immediately - you won't be able to see it again
   - Save the token securely

4. **Get Your Pipedrive Domain**
   - Your domain is in the format: `your-company.pipedrive.com`
   - You can find it in your Pipedrive URL when logged in
   - Example: If your URL is `https://acme.pipedrive.com`, your domain is `acme.pipedrive.com`

### Setting Environment Variables

Add the following to your `.env` file in the **project root**:

```bash
# Pipedrive API Configuration
PIPEDRIVE_API_TOKEN=your_api_token_here
PIPEDRIVE_DOMAIN=your-company.pipedrive.com
```

**Example:**
```bash
PIPEDRIVE_API_TOKEN=abc123def456ghi789jkl012mno345pqr678stu901vwx234yz
PIPEDRIVE_DOMAIN=acme.pipedrive.com
```

### Optional Environment Variables

You can also configure rate limiting and transport settings:

```bash
# Rate Limiting (optional)
PIPEDRIVE_RATE_LIMIT_MIN_TIME_MS=250
PIPEDRIVE_RATE_LIMIT_MAX_CONCURRENT=2

# Transport Configuration (optional, defaults to stdio)
MCP_TRANSPORT=stdio
MCP_PORT=3000
MCP_ENDPOINT=/message
```

---

## Connecting to OI OS

After configuring authentication, connect the server:

```bash
# Load environment variables
export $(grep -E "^PIPEDRIVE" .env | xargs)

# Connect the server
./brain-trust4 connect pipedrive-mcp-server-extended node -- "$(pwd)/MCP-servers/pipedrive-mcp-server-extended/build/index.js"
```

**Note:** The `brain-trust4` command automatically loads the `.env` file from the project root, but explicitly exporting variables ensures they're available.

---

## Creating Intent Mappings

Intent mappings connect natural language queries to specific Pipedrive MCP tools. Create them using SQL INSERT statements.

### Database Location

```bash
sqlite3 brain-trust4.db
```

### All Pipedrive MCP Server Intent Mappings

Run this optimized single SQL statement to create all intent mappings:

```sql
BEGIN TRANSACTION;

-- Intent mappings for Pipedrive MCP server (all 19 tools)
INSERT OR REPLACE INTO intent_mappings (keyword, server_name, tool_name, priority) VALUES 
-- Read operations
('pipedrive get users', 'pipedrive-mcp-server-extended', 'get-users', 10),
('pipedrive users', 'pipedrive-mcp-server-extended', 'get-users', 10),
('pipedrive list users', 'pipedrive-mcp-server-extended', 'get-users', 10),
('pipedrive get deals', 'pipedrive-mcp-server-extended', 'get-deals', 10),
('pipedrive deals', 'pipedrive-mcp-server-extended', 'get-deals', 10),
('pipedrive list deals', 'pipedrive-mcp-server-extended', 'get-deals', 10),
('pipedrive get deal', 'pipedrive-mcp-server-extended', 'get-deal', 10),
('pipedrive deal', 'pipedrive-mcp-server-extended', 'get-deal', 10),
('pipedrive get deal notes', 'pipedrive-mcp-server-extended', 'get-deal-notes', 10),
('pipedrive deal notes', 'pipedrive-mcp-server-extended', 'get-deal-notes', 10),
('pipedrive search deals', 'pipedrive-mcp-server-extended', 'search-deals', 10),
('pipedrive get persons', 'pipedrive-mcp-server-extended', 'get-persons', 10),
('pipedrive persons', 'pipedrive-mcp-server-extended', 'get-persons', 10),
('pipedrive contacts', 'pipedrive-mcp-server-extended', 'get-persons', 10),
('pipedrive list persons', 'pipedrive-mcp-server-extended', 'get-persons', 10),
('pipedrive get person', 'pipedrive-mcp-server-extended', 'get-person', 10),
('pipedrive person', 'pipedrive-mcp-server-extended', 'get-person', 10),
('pipedrive search persons', 'pipedrive-mcp-server-extended', 'search-persons', 10),
('pipedrive search contacts', 'pipedrive-mcp-server-extended', 'search-persons', 10),
('pipedrive get organizations', 'pipedrive-mcp-server-extended', 'get-organizations', 10),
('pipedrive organizations', 'pipedrive-mcp-server-extended', 'get-organizations', 10),
('pipedrive companies', 'pipedrive-mcp-server-extended', 'get-organizations', 10),
('pipedrive list organizations', 'pipedrive-mcp-server-extended', 'get-organizations', 10),
('pipedrive get organization', 'pipedrive-mcp-server-extended', 'get-organization', 10),
('pipedrive organization', 'pipedrive-mcp-server-extended', 'get-organization', 10),
('pipedrive search organizations', 'pipedrive-mcp-server-extended', 'search-organizations', 10),
('pipedrive get pipelines', 'pipedrive-mcp-server-extended', 'get-pipelines', 10),
('pipedrive pipelines', 'pipedrive-mcp-server-extended', 'get-pipelines', 10),
('pipedrive list pipelines', 'pipedrive-mcp-server-extended', 'get-pipelines', 10),
('pipedrive get pipeline', 'pipedrive-mcp-server-extended', 'get-pipeline', 10),
('pipedrive pipeline', 'pipedrive-mcp-server-extended', 'get-pipeline', 10),
('pipedrive get stages', 'pipedrive-mcp-server-extended', 'get-stages', 10),
('pipedrive stages', 'pipedrive-mcp-server-extended', 'get-stages', 10),
('pipedrive search leads', 'pipedrive-mcp-server-extended', 'search-leads', 10),
('pipedrive search all', 'pipedrive-mcp-server-extended', 'search-all', 10),
-- Write operations
('pipedrive create deal', 'pipedrive-mcp-server-extended', 'create-deal', 10),
('pipedrive add deal', 'pipedrive-mcp-server-extended', 'create-deal', 10),
('pipedrive new deal', 'pipedrive-mcp-server-extended', 'create-deal', 10),
('pipedrive create person', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive add person', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive new person', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive create contact', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive add contact', 'pipedrive-mcp-server-extended', 'create-person', 10),
('pipedrive create organization', 'pipedrive-mcp-server-extended', 'create-organization', 10),
('pipedrive add organization', 'pipedrive-mcp-server-extended', 'create-organization', 10),
('pipedrive new organization', 'pipedrive-mcp-server-extended', 'create-organization', 10),
('pipedrive create company', 'pipedrive-mcp-server-extended', 'create-organization', 10),
('pipedrive add company', 'pipedrive-mcp-server-extended', 'create-organization', 10);

COMMIT;
```

### Verifying Intent Mappings

```bash
# List all Pipedrive intent mappings
sqlite3 brain-trust4.db "SELECT * FROM intent_mappings WHERE server_name = 'pipedrive-mcp-server-extended' ORDER BY priority DESC;"

# Or use OI command
./oi intent list | grep pipedrive
```

---

## Creating Parameter Rules

**⚠️ CRITICAL: Parameter rules must be created in the database for parameter extraction to work.**

Parameter rules define which fields are required and how to extract them from natural language queries. The OI OS parameter engine **only extracts required fields** - optional fields are skipped even if extractors exist.

### Why Parameter Rules Are Needed

- **Required fields are extracted**: The parameter engine processes required fields and invokes their extractors
- **Optional fields are skipped**: Optional fields are ignored during parameter extraction, even if extractors exist
- **Database-driven**: Parameter rules are stored in the `parameter_rules` table in `brain-trust4.db`

### Creating Parameter Rules

**Read Operations:** Most read operations have no required parameters (they accept empty objects `{}`):

```sql
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('pipedrive-mcp-server-extended', 'get-users', 'pipedrive-mcp-server-extended::get-users', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-deals', 'pipedrive-mcp-server-extended::get-deals', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-persons', 'pipedrive-mcp-server-extended::get-persons', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-organizations', 'pipedrive-mcp-server-extended::get-organizations', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-pipelines', 'pipedrive-mcp-server-extended::get-pipelines', '[]', '{}', '[]'),
('pipedrive-mcp-server-extended', 'get-stages', 'pipedrive-mcp-server-extended::get-stages', '[]', '{}', '[]');
```

**Read Operations with Required IDs:**

```sql
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('pipedrive-mcp-server-extended', 'get-deal', 'pipedrive-mcp-server-extended::get-deal', '["dealId"]',
 '{"dealId": {"FromQuery": "pipedrive-mcp-server-extended::get-deal.dealId"}}', '[]'),
('pipedrive-mcp-server-extended', 'get-deal-notes', 'pipedrive-mcp-server-extended::get-deal-notes', '["dealId"]',
 '{"dealId": {"FromQuery": "pipedrive-mcp-server-extended::get-deal-notes.dealId"}}', '[]'),
('pipedrive-mcp-server-extended', 'get-person', 'pipedrive-mcp-server-extended::get-person', '["personId"]',
 '{"personId": {"FromQuery": "pipedrive-mcp-server-extended::get-person.personId"}}', '[]'),
('pipedrive-mcp-server-extended', 'get-organization', 'pipedrive-mcp-server-extended::get-organization', '["organizationId"]',
 '{"organizationId": {"FromQuery": "pipedrive-mcp-server-extended::get-organization.organizationId"}}', '[]'),
('pipedrive-mcp-server-extended', 'get-pipeline', 'pipedrive-mcp-server-extended::get-pipeline', '["pipelineId"]',
 '{"pipelineId": {"FromQuery": "pipedrive-mcp-server-extended::get-pipeline.pipelineId"}}', '[]');
```

**Search Operations with Required Term:**

```sql
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('pipedrive-mcp-server-extended', 'search-deals', 'pipedrive-mcp-server-extended::search-deals', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-deals.term"}}', '[]'),
('pipedrive-mcp-server-extended', 'search-persons', 'pipedrive-mcp-server-extended::search-persons', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-persons.term"}}', '[]'),
('pipedrive-mcp-server-extended', 'search-organizations', 'pipedrive-mcp-server-extended::search-organizations', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-organizations.term"}}', '[]'),
('pipedrive-mcp-server-extended', 'search-leads', 'pipedrive-mcp-server-extended::search-leads', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-leads.term"}}', '[]'),
('pipedrive-mcp-server-extended', 'search-all', 'pipedrive-mcp-server-extended::search-all', '["term"]',
 '{"term": {"FromQuery": "pipedrive-mcp-server-extended::search-all.term"}}', '[]');
```

**Create Operations:** Create operations have required fields that need to be extracted from natural language:

```sql
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('pipedrive-mcp-server-extended', 'create-deal', 'pipedrive-mcp-server-extended::create-deal', '["title", "stageId", "ownerId"]',
 '{"title": {"FromQuery": "pipedrive-mcp-server-extended::create-deal.title"}, "stageId": {"FromQuery": "pipedrive-mcp-server-extended::create-deal.stageId"}, "ownerId": {"FromQuery": "pipedrive-mcp-server-extended::create-deal.ownerId"}}', '[]'),
('pipedrive-mcp-server-extended', 'create-person', 'pipedrive-mcp-server-extended::create-person', '["name"]',
 '{"name": {"FromQuery": "pipedrive-mcp-server-extended::create-person.name"}}', '[]'),
('pipedrive-mcp-server-extended', 'create-organization', 'pipedrive-mcp-server-extended::create-organization', '["name"]',
 '{"name": {"FromQuery": "pipedrive-mcp-server-extended::create-organization.name"}}', '[]');
```

### Verifying Parameter Rules

```bash
# List all Pipedrive parameter rules
sqlite3 brain-trust4.db "SELECT tool_signature, required_fields FROM parameter_rules WHERE server_name = 'pipedrive-mcp-server-extended';"

# Check specific tool rule
sqlite3 brain-trust4.db "SELECT * FROM parameter_rules WHERE tool_signature = 'pipedrive-mcp-server-extended::create-deal';"
```

---

## End User Setup

For end users (non-AI agents), follow these steps:

1. **Install the server** (if not already installed):
   ```bash
   ./oi install https://github.com/OI-OS/pipedrive-mcp-server-extended.git
   ```

2. **Build the project**:
   ```bash
   cd MCP-servers/pipedrive-mcp-server-extended
   npm install
   npm run build
   cd ../../
   ```

3. **Get Pipedrive API credentials** (see [Configuring Authentication](#configuring-authentication))

4. **Add credentials to `.env` file** in the project root:
   ```bash
   PIPEDRIVE_API_TOKEN=your_api_token_here
   PIPEDRIVE_DOMAIN=your-company.pipedrive.com
   ```

5. **Connect the server**:
   ```bash
   export $(grep -E "^PIPEDRIVE" .env | xargs)
   ./brain-trust4 connect pipedrive-mcp-server-extended node -- "$(pwd)/MCP-servers/pipedrive-mcp-server-extended/build/index.js"
   ```

6. **Test the connection**:
   ```bash
   ./oi "pipedrive get users"
   ```

---

## Verification & Testing

### Test Direct Tool Calls

```bash
# Test get-users (no parameters)
./brain-trust4 call pipedrive-mcp-server-extended get-users '{}'

# Test get-deals (no parameters)
./brain-trust4 call pipedrive-mcp-server-extended get-deals '{}'

# Test get-deal (with dealId)
./brain-trust4 call pipedrive-mcp-server-extended get-deal '{"dealId": 123}'

# Test search-deals (with term)
./brain-trust4 call pipedrive-mcp-server-extended search-deals '{"term": "test"}'

# Test create-person (with name)
./brain-trust4 call pipedrive-mcp-server-extended create-person '{"name": "John Doe"}'
```

### Test Natural Language Queries

```bash
# Test intent mapping
./oi "pipedrive get users"
./oi "pipedrive get deals"
./oi "pipedrive search deals test"
./oi "pipedrive create person John Doe"
```

### Verify Server Status

```bash
# List all connected servers
./oi list | grep pipedrive

# Check server health
./oi health-check | grep pipedrive
```

---

## Troubleshooting

### Error: "PIPEDRIVE_API_TOKEN environment variable is required"

**Solution:** Ensure your `.env` file contains `PIPEDRIVE_API_TOKEN` and `PIPEDRIVE_DOMAIN`, and that you've exported them before connecting:

```bash
export $(grep -E "^PIPEDRIVE" .env | xargs)
./brain-trust4 connect pipedrive-mcp-server-extended node -- "$(pwd)/MCP-servers/pipedrive-mcp-server-extended/build/index.js"
```

### Error: "PIPEDRIVE_DOMAIN environment variable is required"

**Solution:** Add your Pipedrive domain to `.env`:
```bash
PIPEDRIVE_DOMAIN=your-company.pipedrive.com
```

### Error: "401 Unauthorized" or "403 Forbidden"

**Solution:** 
- Verify your API token is correct
- Check that your API token hasn't expired
- Ensure your Pipedrive account has API access enabled
- Verify your domain is correct (format: `your-company.pipedrive.com`)

### Error: "Cannot find module" or build errors

**Solution:** Rebuild the project:
```bash
cd MCP-servers/pipedrive-mcp-server-extended
rm -rf node_modules build
npm install
npm run build
cd ../../
```

### Natural Language Queries Not Working

**Solution:**
1. Verify intent mappings are created:
   ```bash
   sqlite3 brain-trust4.db "SELECT * FROM intent_mappings WHERE server_name = 'pipedrive-mcp-server-extended';"
   ```

2. Verify parameter rules are created:
   ```bash
   sqlite3 brain-trust4.db "SELECT * FROM parameter_rules WHERE server_name = 'pipedrive-mcp-server-extended';"
   ```

3. Use direct calls instead (more reliable for AI agents):
   ```bash
   ./brain-trust4 call pipedrive-mcp-server-extended get-deals '{}'
   ```

---

## Available Tools Reference

### Read Operations

- **`get-users`**: Get all users/owners from Pipedrive to identify owner IDs for filtering
  - Parameters: None
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-users '{}'`

- **`get-deals`**: Get deals with flexible filtering options
  - Parameters: Optional (searchTitle, daysBack, ownerId, stageId, status, pipelineId, minValue, maxValue, limit)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-deals '{}'`

- **`get-deal`**: Get a specific deal by ID
  - Parameters: `dealId` (required)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-deal '{"dealId": 123}'`

- **`get-deal-notes`**: Get detailed notes and custom booking details for a specific deal
  - Parameters: `dealId` (required), `limit` (optional)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-deal-notes '{"dealId": 123}'`

- **`search-deals`**: Search deals by term
  - Parameters: `term` (required)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended search-deals '{"term": "test"}'`

- **`get-persons`**: Get all persons from Pipedrive
  - Parameters: None
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-persons '{}'`

- **`get-person`**: Get a specific person by ID
  - Parameters: `personId` (required)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-person '{"personId": 456}'`

- **`search-persons`**: Search persons by term
  - Parameters: `term` (required)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended search-persons '{"term": "john"}'`

- **`get-organizations`**: Get all organizations from Pipedrive
  - Parameters: None
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-organizations '{}'`

- **`get-organization`**: Get a specific organization by ID
  - Parameters: `organizationId` (required)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-organization '{"organizationId": 789}'`

- **`search-organizations`**: Search organizations by term
  - Parameters: `term` (required)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended search-organizations '{"term": "acme"}'`

- **`get-pipelines`**: Get all pipelines from Pipedrive
  - Parameters: None
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-pipelines '{}'`

- **`get-pipeline`**: Get a specific pipeline by ID
  - Parameters: `pipelineId` (required)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-pipeline '{"pipelineId": 1}'`

- **`get-stages`**: Get all stages from all pipelines
  - Parameters: None
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended get-stages '{}'`

- **`search-leads`**: Search leads by term
  - Parameters: `term` (required)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended search-leads '{"term": "lead"}'`

- **`search-all`**: Search across all item types
  - Parameters: `term` (required), `itemTypes` (optional)
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended search-all '{"term": "test"}'`

### Write Operations

- **`create-deal`**: Create a new deal in Pipedrive
  - Required Parameters: `title`, `stageId`, `ownerId`
  - Optional Parameters: `value`, `currency`, `personId`, `organizationId`, `status`
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended create-deal '{"title": "New Deal", "stageId": 1, "ownerId": 1}'`

- **`create-person`**: Create a new person/contact in Pipedrive
  - Required Parameters: `name`
  - Optional Parameters: `email`, `phone`, `organizationId`, `ownerId`
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended create-person '{"name": "John Doe", "email": "john@example.com"}'`

- **`create-organization`**: Create a new organization/company in Pipedrive
  - Required Parameters: `name`
  - Optional Parameters: `address`, `city`, `state`, `country`, `zip`, `phone`, `email`, `ownerId`
  - Example: `./brain-trust4 call pipedrive-mcp-server-extended create-organization '{"name": "Acme Corp", "city": "New York"}'`

---

## Parameter Extractors

Parameter extractors are configured in `parameter_extractors.toml.default`. The following extractors are available for Pipedrive tools:

- **Numeric IDs**: `dealId`, `personId`, `organizationId`, `pipelineId`, `stageId`, `ownerId`
- **Search terms**: `term` (for all search operations)
- **Deal creation**: `title`, `stageId`, `ownerId`, `value`, `currency`, `personId`, `organizationId`, `status`
- **Person creation**: `name`, `email`, `phone`, `organizationId`, `ownerId`
- **Organization creation**: `name`, `address`, `city`, `state`, `country`, `zip`, `phone`, `email`, `ownerId`

See `parameter_extractors.toml.default` for the complete list of extraction patterns.

---

**Repository:** https://github.com/OI-OS/pipedrive-mcp-server-extended

**Original Repository:** https://github.com/WillDent/pipedrive-mcp-server

