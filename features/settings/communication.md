---
title: Communication
description: Configure notifications and messages
icon: MessageSquare
order: 3
---

# Communication Settings

Configure how your team communicates with customers.

## Quick Replies

Create reusable message snippets for faster responses.

**Navigation**: Settings -> Quick Reply

### Creating Quick Replies

1. Go to **Settings** -> **Quick Reply**
2. Click **Add Quick Reply**
3. Configure:
   - **Shortcut** - Trigger text (e.g., /hello)
   - **Title** - Display name
   - **Message** - Full message content
4. Save

### Quick Reply Example

| Shortcut  | Message                                                              |
| --------- | -------------------------------------------------------------------- |
| `/hello`  | Hi! Welcome to our support. How can I help you today?                |
| `/hours`  | Our business hours are Mon-Fri 9 AM to 6 PM IST.                     |
| `/thanks` | Thank you for contacting us! Is there anything else I can help with? |
| `/order`  | Please share your order ID and I'll check the status for you.        |

### Using Quick Replies

In chat:

1. Type the shortcut (e.g., `/hello`)
2. Press **Tab** or **Enter** to insert
3. Edit if needed
4. Send

Or:

1. Press `Ctrl + /`
2. Search quick replies
3. Click to insert

### Variables in Quick Replies

Use placeholders for dynamic content:

```
Hi {{name}},

Your order {{order_id}} has been shipped!
Track it here: {{tracking_link}}

Thanks,
{{agent_name}}
```

Available variables:

- `{{name}}` - Customer name
- `{{agent_name}}` - Agent's name
- `{{date}}` - Current date
- `{{time}}` - Current time

### Organizing Quick Replies

Group by category:

- **Greetings** - Welcome messages
- **Orders** - Order-related responses
- **Support** - Common issues
- **Closing** - Conversation endings

## Auto-Assign

Automatically assign conversations to agents.

**Navigation**: Settings -> Auto Assign

### Assignment Modes

| Mode            | Description                   | Best For            |
| --------------- | ----------------------------- | ------------------- |
| **Round Robin** | Distribute evenly in rotation | Equal workload      |
| **Load Based**  | Assign to least busy agent    | Efficiency          |
| **Skill Based** | Match by tags/skills          | Specialized support |
| **Manual**      | No auto-assign                | Full control        |

### Round Robin Setup

1. Select **Round Robin**
2. Choose participating agents
3. Set availability rules
4. Enable

Conversations are assigned in rotation regardless of workload.

### Load Based Setup

1. Select **Load Based**
2. Set max conversations per agent
3. Define "busy" threshold
4. Enable

New chats go to agents with fewest open conversations.

### Skill Based Setup

1. Select **Skill Based**
2. Define skills/tags for agents:
   - Agent A: `sales`, `english`
   - Agent B: `support`, `hindi`
3. Create matching rules:
   - Tag `purchase` -> `sales` skill
   - Tag `issue` -> `support` skill
4. Enable

### Availability Rules

Control when agents receive assignments:

| Setting            | Description                    |
| ------------------ | ------------------------------ |
| **Online Only**    | Only assign to online agents   |
| **Business Hours** | Respect agent schedules        |
| **Max Chats**      | Limit concurrent conversations |
| **Overflow**       | Fallback when all busy         |

### Assignment Queue

When all agents are busy:

1. **Queue** - Hold until agent available
2. **Overflow** - Assign to backup team
3. **Bot** - Hand to chatbot
4. **Leave Unassigned** - Mark for manual pickup

## Read Receipts

Control blue tick visibility.

**Navigation**: Settings -> Read Receipts

### Options

| Setting       | Description                     |
| ------------- | ------------------------------- |
| **Enabled**   | Show read receipts to customers |
| **Disabled**  | Never show read receipts        |
| **Per Agent** | Let agents choose               |

### When to Disable

- Support teams with SLAs
- High-volume operations
- Privacy-conscious industries

## Opt-Out Configuration

Manage how contacts opt out of messages.

**Navigation**: Settings -> Opt-Out Conditions

### Automatic Opt-Out

Triggers that auto-opt-out contacts:

| Trigger             | Default      |
| ------------------- | ------------ |
| Reply "STOP"        | Enabled      |
| Reply "UNSUBSCRIBE" | Enabled      |
| Block your number   | Enabled      |
| Multiple bounces    | Configurable |

### Custom Keywords

Add custom opt-out triggers:

1. Go to **Opt-Out Conditions**
2. Add keywords like:
   - "opt out"
   - "remove me"
   - "cancel subscription"
3. Save

### Opt-Out Confirmation

Send confirmation when opted out:

```
You've been unsubscribed from our messages.
Reply "START" to subscribe again.
```

## Greeting Message

Automatic welcome for new conversations.

### Setting Up Greeting

1. Go to **WhatsApp Profile** -> **Greeting**
2. Enable greeting message
3. Write your welcome text
4. Set trigger conditions

### Greeting Example

```
Welcome to [Business Name]!

How can we help you today?

1. Order Status
2. Product Info
3. Support
4. Other

Reply with the number or type your question.
```

### Trigger Conditions

| Condition       | Description                 |
| --------------- | --------------------------- |
| **New Contact** | First message ever          |
| **14-Day Gap**  | No messages for 14 days     |
| **Always**      | Every new conversation      |
| **Off Hours**   | Only outside business hours |

## Away Message

Auto-reply when unavailable.

### Setting Up Away Message

1. Go to **WhatsApp Profile** -> **Away Message**
2. Enable away message
3. Write your response
4. Set schedule

### Away Message Example

```
Thanks for reaching out!

We're currently closed. Our business hours are:
Mon-Fri: 9 AM - 6 PM IST

We'll respond when we're back.
For urgent matters, email support@example.com
```

### Schedule Options

| Option              | Description                |
| ------------------- | -------------------------- |
| **Always**          | Send whenever away enabled |
| **Business Hours**  | Send outside set hours     |
| **Custom Schedule** | Define specific times      |

## Template Link Tracker

Automatically shorten URLs in your template messages and track clicks with analytics.

**Navigation**: Settings -> **Template Link Tracker**

---

### Interface Layout

The page has two controls:

1. **Enable/Disable toggle** - Turn link tracking on or off for your business
2. **Custom Domain URL** - Register your own branded domain for short links

---

### Enabling Link Tracking

1. Go to **Settings** -> **Template Link Tracker**
2. Toggle the switch **ON** in the top-right corner
3. When enabled, any template with a link tracker URL type will automatically shorten URLs when sent

> [!NOTE]
> When link tracking is disabled, URLs in templates are sent as-is without shortening, even if the template was created with a link tracker URL type.

---

### How It Works

When you create a template with a **URL button**, you choose a URL type:

| URL Type                       | Description                            | Example URL                     |
| ------------------------------ | -------------------------------------- | ------------------------------- |
| **Static**                     | Fixed URL, no shortening               | `https://www.example.com/page`  |
| **Dynamic**                    | URL with a variable suffix             | `https://www.example.com/{{1}}` |
| **Link Tracker (API domain)**  | Auto-shortened via Heltar's domain     | `https://api.heltar.com/{{1}}`  |
| **Custom Domain Link Tracker** | Auto-shortened via your branded domain | `https://yourdomain.com/{{1}}`  |

When you select **Link Tracker** or **Custom Domain Link Tracker**:

- The URL field is auto-filled and read-only
- You provide the actual destination URL at send time (not during template creation)
- Each send generates a unique short link that redirects to your destination
- Every click is tracked with analytics (browser, device, location)

> [!TIP]
> The "Custom domain URL link tracker" option only appears after you register a custom domain.

---

### Setting Up a Custom Domain

A custom domain lets you use your own branded short links (e.g., `https://links.yourbrand.com/abc123`) instead of the default Heltar API domain.

1. Go to **Settings** -> **Template Link Tracker**
2. Enter your custom domain URL (e.g., `https://links.yourbrand.com`)
3. Click **Register Domain**
4. **Contact Heltar support** to complete domain verification and DNS setup

> [!IMPORTANT]
> After registering, you need to point your domain to Heltar's servers. Contact support for the DNS configuration details. The custom domain will not work until this setup is complete.

---

### Click Analytics

Every shortened link tracks:

| Data Point    | Description                   |
| ------------- | ----------------------------- |
| **Browser**   | Chrome, Safari, Firefox, etc. |
| **OS**        | iOS, Android, Windows, etc.   |
| **Device**    | Mobile, Desktop, Tablet       |
| **Location**  | City, region, country         |
| **Timestamp** | When the link was clicked     |
| **Recipient** | Which contact clicked         |

View analytics in the **Link Tracker** integration (Integrations -> Link Tracker -> Open).

---

### Creating a Template with Link Tracking

1. Go to **Settings** -> **Template Manager**
2. Create or edit a template
3. Add a **URL button**
4. In the URL Type dropdown, select **Link Tracker (API domain)** or **Custom domain URL link tracker**
5. Add a sample URL for Meta's review (auto-filled for link tracker types)
6. Submit the template for approval

Once approved, every time you send this template or run a campaign, the URL is automatically shortened and clicks are tracked.

---

## Best Practices

1. **Keep quick replies updated** - Review monthly
2. **Test auto-assign** - Verify distribution is fair
3. **Monitor response times** - Adjust settings accordingly
4. **Personalize greetings** - Make them warm and helpful
5. **Clear away messages** - Set expectations correctly
