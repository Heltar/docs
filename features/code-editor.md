---
title: Code Editor
description: Write custom functions
icon: Code
order: 7
---

# Code Editor

Code Editor is a cloud-based IDE where you can create AI chatbots and custom functions. It uses a virtual folder structure to organize your chatbot configurations and function definitions.

**Navigation**: Click **Code Editor** in the left sidebar

---

## Interface Layout

### Left Panel - File Explorer

- Virtual folder tree structure
- Collapsible folders
- Right-click context menu for file operations

### Center Panel - Editor

- Monaco Editor (same as VS Code)
- Syntax highlighting for JSON and Python
- Auto-save support

### Right Panel - Copilot (Optional)

- AI assistant for code generation
- Click the **sparkle icon** to open

### Top Toolbar

- **Save** button
- **Test Run** button
- **Create Version** button
- **Deploy** dropdown
- **Settings** (gear icon)

---

## Virtual Folder Structure

When you open Code Editor, you'll see:

```
/
├── all_events_entry_point.py    <- Main Python handler
├── chatbot/                      <- AI chatbot configurations
│   └── my_bot.json
└── function_definitions/         <- Tool/function definitions
    └── get_order.json
```

| Folder                      | Purpose                                         |
| --------------------------- | ----------------------------------------------- |
| `chatbot/`                  | JSON files defining AI chatbots                 |
| `function_definitions/`     | JSON files defining callable functions          |
| `all_events_entry_point.py` | Python code that runs when functions are called |

---

## Creating a Chatbot

Chatbots are JSON files that define an AI assistant configuration.

### Step 1: Create the File

1. Right-click on the **chatbot** folder
2. Click **New File**
3. Name it like `support_bot.json`

### Step 2: Add Configuration

Paste this template (replace UUID with your own):

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "support_bot",
  "model": "gpt-4.1",
  "systemPrompt": "You are a helpful customer support assistant. Be friendly and concise.",
  "temperature": 0.7
}
```

### Step 3: Save

Click **Save** or press **Ctrl+S**.

---

## Chatbot Configuration Options

| Field             | Required | Description                                 |
| ----------------- | -------- | ------------------------------------------- |
| `id`              | Yes      | UUID for the chatbot (must be unique)       |
| `name`            | Yes      | Unique name for your chatbot                |
| `model`           | No       | AI model to use. Default: gpt-5.1           |
| `systemPrompt`    | No       | Instructions for the AI                     |
| `temperature`     | No       | Creativity level (0.0 to 1.0). Default: 0.7 |
| `maxTokens`       | No       | Max response tokens. Default: 1024          |
| `contextLength`   | No       | Messages to include. Default: 100           |
| `hasTimeout`      | No       | Enable timeout. Default: false              |
| `timeoutDuration` | No       | Timeout in seconds (if hasTimeout is true)  |
| `functions`       | No       | Array of function names the bot can call    |

> [!IMPORTANT]
> The `id` field is required and must be a valid UUID. This ID is used to sync the chatbot between Code Editor and the AI Agent page. Use a UUID generator or let Copilot generate one for you.

### Available Models

| Model          | Best For                     |
| -------------- | ---------------------------- |
| `gpt-4.1`      | General purpose, balanced    |
| `gpt-4.1-nano` | Fast responses, simple tasks |
| `gpt-5`        | Most capable, complex tasks  |

### Chatbot with Functions

```json
{
  "id": "660e8400-e29b-41d4-a716-446655440001",
  "name": "order_assistant",
  "model": "gpt-4.1",
  "systemPrompt": "You help customers track their orders. Use the get_order_status function when they ask about an order.",
  "temperature": 0.7,
  "functions": ["get_order_status", "check_inventory"]
}
```

---

## Creating Functions

Functions are tools that your chatbot can call to perform actions.

### Step 1: Create Function Definition

1. Right-click on **function_definitions** folder
2. Click **New File**
3. Name it like `get_order_status.json`

### Step 2: Define the Function

```json
{
  "name": "get_order_status",
  "type": "function",
  "description": "Gets the current status of a customer order",
  "parameters": {
    "type": "object",
    "properties": {
      "order_id": {
        "type": "string",
        "description": "The order ID like ORD-12345"
      }
    },
    "required": ["order_id"]
  }
}
```

### Step 3: Write the Handler

Open `all_events_entry_point.py` and add your logic:

```python
def all_events_handler(event, context):
    function_name = event.get('function_name')

    if function_name == 'get_order_status':
        order_id = event.get('order_id')
        # Your logic here - call your API, database, etc.
        return {
            "status": "shipped",
            "tracking_number": "TRK123456",
            "estimated_delivery": "Jan 25, 2024"
        }

    return {"error": f"Unknown function: {function_name}"}
```

### Step 4: Link to Chatbot

In your chatbot JSON, add the function:

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "support_bot",
  "model": "gpt-4.1",
  "systemPrompt": "You help customers track their orders.",
  "functions": ["get_order_status"]
}
```

---

## Function Definition Format

```json
{
  "name": "function_name",
  "type": "function",
  "description": "What this function does",
  "parameters": {
    "type": "object",
    "properties": {
      "param_name": {
        "type": "string",
        "description": "Parameter description"
      }
    },
    "required": ["param_name"]
  }
}
```

### Parameter Types

| Type      | Example        |
| --------- | -------------- |
| `string`  | Text values    |
| `number`  | Numeric values |
| `boolean` | true/false     |
| `array`   | List of values |
| `object`  | Nested object  |

---

## Testing Your Code

### Test Run

1. Click **Test Run** button in toolbar
2. Enter a test payload (JSON):

```json
{
  "function_name": "get_order_status",
  "order_id": "ORD-12345"
}
```

3. Click **Run**
4. See output in the console — includes the function result, print logs, and status code

> [!TIP]
> Test Run always executes the latest draft (`$LATEST`) code, not the deployed version. This lets you iterate quickly without deploying.

### Docs Tab

The Run Panel has a **Docs** tab that shows:

- **How to run your code** — the request payload format (`function_name` + parameters)
- **API Endpoint** — `POST /v1/org/code/run` for running deployed code externally
- **Authentication** — Bearer token from your API key (generate in **Settings** → **Developer**)
- **What happens automatically** — your actual business details that get injected into every request (you don't need to include them)
- **Example cURL** — a ready-to-copy cURL command (auto-injected fields are stripped)
- **Response format** — what you get back (`result`, `logs`, `statusCode`)

> [!TIP]
> Use the Docs tab to quickly copy a cURL command for testing your deployed code from the terminal or integrating with external systems.

---

## Deploying

### Create a Version

1. Click **Create Version** in toolbar
2. Add a description (e.g., "Added order tracking")
3. Click **Create**

### Deploy

1. Click the **Deploy** dropdown
2. Select the version to deploy
3. Confirm deployment

> [!IMPORTANT]
> **Chatbot Sync on Deploy**: When you deploy a version, all chatbots and functions defined in your Code Editor are automatically synced to the database. This means:
>
> - New chatbots appear on the **AI Agent** page
> - Existing chatbots (matched by `id`) are updated with new configurations
> - Functions are created or updated and linked to chatbots
> - The deployed version number is stored with each chatbot

---

## Chatbot Sync Workflow

When working with chatbots in Code Editor, follow this workflow:

### 1. Create Chatbot in Code Editor

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "support_bot",
  "model": "gpt-4.1",
  "systemPrompt": "You are a helpful assistant.",
  "functions": ["get_order_status"]
}
```

### 2. Create Version

Click **Create Version** to snapshot your current code.

### 3. Deploy Version

Click **Deploy** and select your version. This syncs chatbots to the database.

### 4. View on AI Agent Page

Your chatbot now appears on the **AI Agent** page with:

- A "Sync with code version" badge
- The deployed version number (e.g., "v21")
- A warning icon indicating to edit via Code Editor

### 5. Making Changes

> [!WARNING]
> Chatbots created via Code Editor should be edited in Code Editor to stay in sync. Changes made on the AI Agent page will be overwritten on the next deploy.

To update a Copilot chatbot:

1. Edit the JSON file in Code Editor
2. Create a new version
3. Deploy the new version

The chatbot is updated with the new configuration and version number.

---

## Environment Variables

Store API keys and secrets securely.

### Adding Variables

1. Click **Settings** (gear icon) in toolbar
2. Add your variables:
   - Key: `STORE_API_KEY`
   - Value: `your-secret-key`
3. Click **Save**

### Using in Code

```python
import os

def all_events_handler(event, context):
    api_key = os.environ.get('STORE_API_KEY')
    # Use api_key in your API calls
```

---

## Using Copilot

Code Editor has an AI assistant (Copilot) that helps you write code.

### Open Copilot

Click the **sparkle icon** in the top-right corner.

### What Copilot Can Do

Ask Copilot to:

- **"Create a chatbot for customer support"**
- **"Add a function to check inventory"**
- **"Fix this error in my code"**
- **"Explain what this function does"**

Copilot generates code and shows suggested changes. Click **Accept** to apply.

---

## Quick Reference

### Chatbot Template

```json
{
  "id": "YOUR-UUID-HERE",
  "name": "bot_name",
  "model": "gpt-4.1",
  "systemPrompt": "Your instructions here",
  "temperature": 0.7,
  "functions": []
}
```

> [!TIP]
> Generate a UUID at https://www.uuidgenerator.net/ or ask Copilot to generate one.

### Function Template

```json
{
  "name": "function_name",
  "type": "function",
  "description": "What this function does",
  "parameters": {
    "type": "object",
    "properties": {
      "param_name": {
        "type": "string",
        "description": "Parameter description"
      }
    },
    "required": ["param_name"]
  }
}
```

### Handler Template

```python
def all_events_handler(event, context):
    function_name = event.get('function_name')

    if function_name == 'your_function':
        # Your logic here
        return {"result": "success"}

    return {"error": "Unknown function"}
```

---

## Tips

> [!TIP]
> Start simple. Create a basic chatbot first, then add functions as needed.

> [!WARNING]
> Always use environment variables for API keys and secrets. Never hardcode them.

> [!TIP]
> Use Copilot to generate boilerplate code and save time.

> [!TIP]
> Test each function individually before linking it to a chatbot.
