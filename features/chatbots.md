---
title: Chatbots
description: Automate responses
icon: Bot
order: 5
---

# AI Agent

Create AI-powered chatbots that can handle customer conversations automatically. Configure custom functions, test in the playground, and publish to handle real conversations.

**Navigation**: Click **AI Agent** in the left sidebar

---

## Interface Overview

The AI Agent page has two main views:

1. **Chatbot Dashboard** - Grid of all your chatbots
2. **Chatbot Playground** - Configure and test a specific chatbot

---

## Chatbot Dashboard

### Header

- Search box to find chatbots by name
- **API Key Manager** dropdown (top-right)

### Chatbot Grid

Displays all chatbots as cards in a responsive grid:

- 2 columns on mobile
- 3 columns on large screens

### Chatbot Card Information

Each card shows:

- **Chatbot Name** (with truncation for long names)
- **Model Badge** (e.g., gpt-4.1-nano) - green background
- **Publication Status**:
  - Blue badge with message icon = Published as Text Bot
  - Green badge with phone icon = Published as Voice Bot
- **Created Date** (formatted as "Jan 15, 2024")
- **Function Count** (e.g., "3 functions")
- **Timeout Status** (if enabled)
- **Copilot Badge** (for chatbots created via Code Editor):
  - "Sync with code version" text with CPU icon
  - Version badge (e.g., "v21")
  - Warning icon - indicates editing should be done via Code Editor

### Chatbot Card Menu (Three Dots)

Click the three-dot menu on any card to:

- **Publish as Text Bot** / **Unpublish Text Bot**
- **Publish as Voice Bot** / **Unpublish Voice Bot**
- **Export** - Download as encrypted .hcb file
- **Delete** - Remove the chatbot (red text)

---

## Creating a New Chatbot

### Create New Tab

1. Click the **+** card in the grid
2. The **Create New** tab is selected by default
3. Enter a **Name** for your chatbot
4. Click **Create**

### Import Tab

Import a previously exported chatbot:

1. Click the **+** card
2. Switch to the **Import** tab
3. Drag and drop a **.hcb file** (or click to browse)
4. Enter the **Encryption Key** (provided when exported)
5. Optionally enter a **New Chatbot Name**
6. Click **Import**

---

## Chatbot Playground

Click on any chatbot card to open the Playground.

### Top Navigation Bar

| Element             | Description                    |
| ------------------- | ------------------------------ |
| **Back Arrow**      | Returns to chatbot dashboard   |
| **Chatbot Name**    | Current chatbot name           |
| **Tab Navigation**  | Playground, Results, Follow up |
| **API Key Manager** | Select which API key to use    |
| **Publish Button**  | Publish/unpublish options      |

### Three Tabs

1. **Playground** - Test and configure the chatbot
2. **Results** - View AI analysis of conversations
3. **Follow up** - Manage scheduled follow-up messages (nudges)

---

## Playground Tab

The Playground has two resizable panels.

### Left Panel - Conversation Area

#### Conversation Header

- **Client Phone** input - Optional phone number for context
- **Structured Output** toggle - Enable JSON schema output
- **Auto-Execute Functions** toggle - Run tools automatically
- **Save Chat** button - Save conversation locally
- **Replay** dropdown - Replay saved or live chats
- **New Chat** button - Start fresh conversation

#### Message Display

- **User Messages** - Right-aligned, light green background
- **Assistant Messages** - Left-aligned, light gray background
- **Tool Calls** - Sky blue background, expandable arguments
- **Tool Responses** - Emerald green background, expandable JSON

#### Message Input

- Text area for typing messages
- **Send** button to submit

#### Tool Response Mode

When a tool is called and waiting for response:

- Text area for entering JSON response
- **Cancel** / **Submit** buttons

### Right Panel - Settings

#### Model & Context Section

- **Model Selector** - Choose AI model (gpt-4.1, gpt-5, etc.)
- **Context Length** - Number of messages to include (default: 10)

#### Tools Section

- List of attached functions as badges
- **Direct Return** badge (blue) — function result is sent directly to user without a follow-up LLM call
- **X** button to remove a function
- **+** dropdown to add functions:
  - **Create new function** - Opens Code Editor
  - **Use existing function** - Browse organization library

#### Organization Functions Library

When adding existing functions:

- Search to filter functions
- Shows function type:
  - Blue "API Call" badge
  - Yellow "Python" badge
  - Purple "Unified" badge
- Shows creator:
  - Sparkle icon = AI Generated
  - User icon = Manual
- Checkbox to select multiple
- **Add** button to attach selected functions

#### Function Dialog (Create / Edit)

When creating or editing a function, the dialog shows:

- **Function Definition (JSON)** — OpenAI-format JSON with name, description, and parameters
- **Direct Return** toggle — When enabled, the function result is returned directly to the user without a follow-up LLM call. Useful for pre-formatted responses (carousels, lists, media)
- **Execution Type** tabs:
  - **Unified Event Handlers** (recommended) — Routed through `all_events_handler` in Code Editor
  - **API Call** (deprecated) — Direct HTTP call
  - **Python** (deprecated) — Inline Python code
  - **No Execution** — Schema only, no auto-execution

> [!INFO]
> The `directReturn` flag is preserved when syncing between the AI Agent page and Code Editor. In Code Editor format, it appears in the `functions` array as `{ "name": "...", "directReturn": true }`.

#### System Message Section

- Large text area for system prompt
- **Expand** button (arrow icon) - Full-screen edit
- **Generate** button (sparkles) - AI-generate a system prompt

### Advanced Settings Tab

Additional configuration options:

- **Temperature** (0-2) - Creativity level
- **Max Tokens** - Response length limit
- **Top P** - Nucleus sampling
- **Reasoning Effort** (for GPT-5) - minimal/low/medium/high
- **Verbosity Level** - low/medium/high
- **Message Format** - text/button/ctaUrl
- **Typing Indicator** - Show typing dots
- **Min Bot Delay** - Minimum seconds before response
- **Max Consecutive Nudges** - Follow-up message limit
- **Max Bot Messages** - Messages per session limit

#### Voice Bot Settings (if voice model selected)

- **TTS Provider** - Cartesia/ElevenLabs/OpenAI Realtime
- **STT Provider** - Deepgram/AssemblyAI
- **Voice Selection** - Choose TTS voice
- **Language Selection** - STT language

---

## Test Voice Bot

Every chatbot with voice enabled has a **Test Call** button in the Voice Bot tab. Use it to validate the agent's voice, tone, and answers before publishing — without touching the chatbot flows on real customers.

### How to open it

1. Open a chatbot → **Playground** → **Settings** → **Voice Bot** tab.
2. Click **Test Call** in the top-right of the Voice Bot panel.
3. A dialog opens with three ways to test:

| Mode         | What it does                                            | When to use                                                              |
| ------------ | ------------------------------------------------------- | ------------------------------------------------------------------------ |
| **WhatsApp** | Bot calls your WhatsApp number.                         | End-to-end test of the WhatsApp call path + your user's real experience. |
| **Phone**    | Bot calls your regular phone number via SIP.            | Test SIP trunk setup + phone-network audio quality.                      |
| **Browser**  | Talk to the bot right from this page — no phone needed. | Fastest iteration while tweaking prompts, voice, or tools.               |

### Setup requirements

The dialog shows a warning card for any mode you haven't set up yet — plus a one-click link to the matching setup page:

- **WhatsApp** mode needs WhatsApp Business API onboarding (Settings → **WhatsApp API Setup**).
- **Phone** mode needs an outbound SIP trunk (Settings → **SIP Calling**).
- **Browser** mode always works — just allow microphone access in the browser.

Modes you haven't set up show a small warning dot on the tab, but you can still open each tab to see what's missing and fix it.

### Browser test tips

- Click **Start Call**, then speak normally.
- Use the **Mic/Mute** toggle to pause your mic without ending the call.
- Hit **End** (or close the dialog) to hang up. The test room is disposable — no transcript is written into your customer data.

### WhatsApp / Phone test tips

- Enter your own number with country code, no `+` or spaces (e.g., `919999999999`).
- You'll receive the call within a few seconds. Pick up to start the conversation.
- Normal call actions (mute, hang up) work as they would on a real customer call.

### Troubleshooting

- **"No active voice bot configured"** — Save the chatbot at least once, or set it as the business's active voice bot, before testing.
- **Browser mode shows no audio** — Check browser mic permission for the site; reload and retry.
- **WhatsApp call never arrives** — Confirm WhatsApp API is set up and the number you entered has WhatsApp installed.
- **Phone call says "failed"** — SIP trunk is mis-configured or out of credit. Open Settings → SIP Calling.

---

## Results Tab

View and analyze bot conversation results.

### AI Analysis Section

Configure how to analyze conversations:

- **Model** - Which AI model to use
- **System Prompt** - Instructions for analysis
- **Context Length** - Messages to analyze
- **Temperature** - Analysis creativity
- **Reasoning Effort** (GPT-5)
- **Include Private Messages** checkbox

### JSON Schema Editor

Define the output format for analysis:

- Visual JSON editor
- JSON Converter tool

### Processing

- **Filter** - All Rows / Top 50 / Filtered Rows
- **Concurrency** - Parallel processing count
- **Process** button - Start analysis

### Results Table Columns

- Created At
- Client Name
- Client Number (maskable, copyable)
- Chatbot Name
- Dynamic columns based on your schema

---

## Follow Up Tab (Nudges)

Manage scheduled follow-up messages.

### Table Columns

| Column               | Description                                    |
| -------------------- | ---------------------------------------------- |
| **Client Number**    | Phone number                                   |
| **Chatbot Name**     | Which bot scheduled it                         |
| **Status**           | Draft, Scheduled, In Progress, Sent, Cancelled |
| **Follow-up Type**   | Message type                                   |
| **Follow-up Number** | Which follow-up (1st, 2nd, etc.)               |
| **Delay (s)**        | Seconds until send                             |
| **Replied**          | Yes/No if user responded                       |
| **Scheduled For**    | When it will be sent                           |

### Status Colors

| Status      | Color   |
| ----------- | ------- |
| Draft       | Gray    |
| Scheduled   | Blue    |
| In Progress | Amber   |
| Sent        | Emerald |
| Cancelled   | Rose    |

### Actions

- Click row to preview the nudge in chat panel
- **Delete** button with confirmation dialog

---

## Publishing a Chatbot

### Publish as Text Bot

Makes the chatbot respond to WhatsApp text messages:

1. Open the chatbot Playground
2. Click **Publish** button (top-right)
3. Select **Publish as Text Bot**

### Publish as Voice Bot

Makes the chatbot handle voice calls:

1. Open the chatbot Playground
2. Click **Publish** button
3. Select **Publish as Voice Bot**

> [!INFO]
> Only one chatbot can be active as Text Bot and one as Voice Bot at a time.

### Unpublish

1. Click **Publish** button
2. Select **Unpublish Text Bot** or **Unpublish Voice Bot**

---

## Live Chatbots (Weighted Distribution)

When you have multiple chatbots, you can configure weighted distribution to control how new conversations are assigned.

**Navigation**: Settings -> Bot -> Live Chatbots

### How It Works

- Each live chatbot has a **weight** (0-100)
- New conversations are assigned to chatbots based on their weight proportion
- Already-assigned conversations stay with their current chatbot (**sticky assignment**)

### Setting Weights

1. Go to **Settings** -> **Bot** -> **Live Chatbots**
2. Add chatbots to the live list
3. Adjust the weight slider for each chatbot
4. Save

### Weight Examples

| Chatbot     | Weight | New Conversations |
| ----------- | ------ | ----------------- |
| support_bot | 70     | ~70% of new chats |
| sales_bot   | 30     | ~30% of new chats |

### Pausing a Chatbot (Weight = 0)

Set a chatbot's weight to **0** to stop it from receiving new conversations, while keeping it live for existing ones:

- **New conversations** — will not be assigned to this chatbot
- **Existing conversations** — sticky assignment continues, the chatbot still handles its already-assigned contacts

> [!TIP]
> Use weight 0 to gradually phase out a chatbot. Existing contacts keep their assigned bot, but no new contacts are routed to it.

---

## Export/Import Chatbots

### Export

1. Click three-dot menu on chatbot card
2. Select **Export**
3. Save the .hcb file
4. Copy the encryption key (displayed in dialog)

### Import

1. Click **+** card
2. Select **Import** tab
3. Upload .hcb file
4. Enter encryption key
5. Click **Import**

---

## API Key Manager

Manage API keys for OpenAI:

### Select Key

- Click the **Key** dropdown (top-right)
- Select from available keys
- Active key shows green badge

### Add New Key

1. Click **Add New API Key**
2. Enter name, type, and key value
3. Toggle **Active** status
4. Click **Add**

### Delete Key

- Click **X** next to key (red)
- Default key cannot be deleted

---

## Function Linking Format (Code Editor)

When linking functions to a chatbot via the Code Editor, the `functions` array in the chatbot JSON uses objects:

```json
"functions": [
  { "name": "get_order_status", "directReturn": false },
  { "name": "search_products", "directReturn": false },
  { "name": "send_carousel", "directReturn": true }
]
```

| Field          | Required | Default | Description                                                                  |
| -------------- | -------- | ------- | ---------------------------------------------------------------------------- |
| `name`         | Yes      | —       | Function name (must match a file in `function_definitions/` folder)          |
| `directReturn` | No       | `false` | If `true`, function result is returned directly without a follow-up LLM call |

### When to use `directReturn: true`

- Sending pre-formatted messages (carousels, lists, media)
- Returning raw data that doesn't need AI interpretation
- Functions that handle their own response formatting

### Backward Compatibility

Plain string names still work and are treated as `{ "name": "...", "directReturn": false }`:

```json
"functions": ["get_order_status", "search_products"]
```

---

## Copilot-Created Chatbots

Chatbots can also be created via the **Code Editor** (Copilot). These chatbots have special behavior:

### Identifying Copilot Chatbots

- **"Sync with code version"** text on the card
- **Version badge** showing the deployed version (e.g., "v21")

### Editing Copilot Chatbots

> [!WARNING]
> Changes made on the AI Agent page to Copilot chatbots will be **overwritten** on the next deploy from Code Editor.

To properly edit a Copilot chatbot:

1. Go to **Code Editor**
2. Edit the chatbot JSON file in `chatbot/` folder
3. Click **Create Version**
4. Click **Deploy** and select the new version

The chatbot will be updated with the new configuration and version number.

### When to Use Code Editor vs AI Agent

| Task                                 | Use         |
| ------------------------------------ | ----------- |
| Create chatbot with custom functions | Code Editor |
| Quick test in Playground             | AI Agent    |
| Create chatbot without coding        | AI Agent    |
| Manage functions in Python           | Code Editor |
| Publish/Unpublish bot                | AI Agent    |

---

## Clear Bot Session

Sometimes a chatbot conversation goes off-track, gets stuck in a loop, or accumulates outdated context that confuses the bot. **Clear Bot Session** lets you reset the bot's conversation memory for a specific contact without deleting any messages.

### What It Does

- Sets a "session cleared" marker in the chat timeline
- The bot will only use messages **after** this marker as context for future replies
- All previous chat history remains visible in the inbox — nothing is deleted
- A "Bot session cleared" indicator appears in the chat with timestamp

### When to Use

- Bot is stuck in a repetitive loop or giving irrelevant responses
- Contact's conversation topic has changed completely
- Old context is confusing the bot (e.g., resolved issues being referenced)
- After updating the bot's system prompt and wanting a fresh start for a contact

### How to Clear

1. Open a conversation in the **Inbox**
2. Click **Chat Options** (top-right)
3. Under **Bot Status**, click **Clear Bot Session**
4. The session is cleared immediately — a marker appears in the chat

> [!NOTE]
> Clear Bot Session only affects which messages the bot sees as context. It does not turn the bot on/off, delete messages, or change any other settings.

---

## Best Practices

> [!TIP]
> Start with a clear system prompt that defines the bot's personality and capabilities.

> [!TIP]
> Test thoroughly in the Playground before publishing. Try edge cases and unexpected inputs.

> [!TIP]
> Use functions to give your bot real capabilities like checking order status, booking appointments, etc.

> [!TIP]
> Monitor the Results tab to understand how your bot is performing and where it needs improvement.
