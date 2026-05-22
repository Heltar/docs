---
title: Developer Tools
description: API keys and webhooks
icon: Code
order: 7
---

# Developer Tools

Configure API access and webhooks for integrations.

---

## API Key Management

Generate API keys for programmatic access to the platform.

**Navigation**: Settings -> **API Key/Dev Tools**

---

### Interface Layout

The API Key Management page has three sections:

1. **Generate API Key** - Create new keys with expiry
2. **API Key** - View current key and expiration
3. **Developer Settings** - Enable developer mode

---

### Generating an API Key

1. Go to **Settings** -> **API Key/Dev Tools**
2. In the **Generate API Key** section:
   - Click the date-time picker
   - Select expiry date and time
   - Minimum: 5 minutes from now
   - Maximum: 5 years from now
3. Click **Generate**
4. The new API key appears below

---

### Viewing Your API Key

The **API Key** section shows:

| Field              | Description                                 |
| ------------------ | ------------------------------------------- |
| **Key Expires At** | Expiration date and time                    |
| **API Key**        | The full JWT token (if you have permission) |
| **Copy** button    | Copy key to clipboard                       |

> [!INFO]
> You need the `viewApiKey` permission to see the API key. Contact your admin if you see "You do not have permission to view the API key."

---

### Using the API Key

Include in request headers:

```bash
curl -X GET "{{API_URL}}/v1/clients" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

### Developer Mode

Enable Developer Mode to see all API calls as cURL commands in the browser console.

**To enable:**

1. Toggle **Enable Developer Mode** switch ON
2. Use the app normally (send messages, load contacts, etc.)
3. Right-click anywhere -> click **Inspect**
4. Go to **Console** tab
5. See all API requests logged as cURL commands

**Example console output:**

```bash
curl -X GET "{{API_URL}}/v1/clients?limit=500" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..." \
  -H "Content-Type: application/json"
```

> [!INFO]
> The API URL will match your platform's configured API endpoint.

**Uses:**

- Copy cURL commands and test APIs in terminal
- Debug API integrations
- Share exact API calls with developers
- Replicate requests in Postman or other tools

> [!TIP]
> Developer Mode setting is stored locally in your browser. It persists across sessions.

---

## Webhook Manager

Forward events to your own endpoints.

**Navigation**: Settings -> **Webhook Manager**

---

### Interface Layout

- **Webhook List** - All configured webhooks
- **Add New URL** button - Create new webhook
- **Update** button - Save toggle changes

---

### Webhook List

Each webhook card shows:

| Element                        | Description                |
| ------------------------------ | -------------------------- |
| **Callback URL**               | Your endpoint URL          |
| **Enable/Disable** toggle      | Turn webhook on/off        |
| **Delete** button (trash icon) | Remove webhook             |
| **Webhook fields**             | Badges showing event types |
| **Verify Token**               | Masked as dots (if set)    |

---

### Adding a Webhook

1. Click **Add New URL**
2. Fill in the form:
   - **Callback URL** (required) - Your HTTPS endpoint
   - **Verify Token** (optional) - For verification
   - **Webhook Fields** (required) - Select one event type
3. Click **Add Webhook**

---

### Webhook Field Types

| Field                         | Description                                          |
| ----------------------------- | ---------------------------------------------------- |
| **Meta Webhooks**             | All Meta webhook events forwarded to your URL        |
| **CleverTap Status**          | CleverTap connection status changes                  |
| **CleverTap Messages**        | Meta webhook message events for CleverTap            |
| **WebEngage Status**          | Meta webhook status events for WebEngage             |
| **MoEngage**                  | Incoming messages and delivery status for MoEngage   |
| **Meta Custom Field Hook**    | Meta webhooks with custom fields                     |
| **Bot Webhook**               | All Meta webhooks (skips contacts with bot disabled) |
| **Frontend Campaign Forward** | Bulk campaign payload from frontend                  |

> [!INFO]
> You can only select one field type per webhook. Create multiple webhooks for different event types.

---

### Managing Webhooks

**Enable/Disable:**

1. Toggle the switch next to any webhook
2. Click **Update** to save changes

**Delete:**

1. Click the trash icon
2. Webhook is removed immediately

---

## Webhook Setup Details

View the callback URL and verification token for Meta webhook configuration.

**Navigation**: Settings -> **Webhook Setup Details**

---

### Information Displayed

| Field                      | Value                                   |
| -------------------------- | --------------------------------------- |
| **Web verification token** | Your unique token for Meta verification |
| **Webhook callback URL**   | `{apiUrl}/v1/webhooks/req/{businessId}` |

Use these values when configuring webhooks in the Meta Developer Portal.

---

## Best Practices

> [!WARNING]
> Never expose API keys in frontend code or public repositories.

> [!TIP]
> Use environment variables to store API keys in your server code.

> [!TIP]
> Test webhooks with tools like ngrok before deploying to production.

> [!TIP]
> Regenerate API keys periodically for security.
