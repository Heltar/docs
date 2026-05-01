---
title: API Reference
description: Integrate with our API
icon: Code
order: 1
---

# API Reference

Build powerful WhatsApp integrations with our REST API. Send messages, manage contacts, run campaigns, and receive real-time webhooks.

## Base URL

All API requests should be made to:

```
{{API_URL}}/v1
```

## Authentication

Include your API key in the `Authorization` header with every request:

```bash
Authorization: Bearer YOUR_API_KEY
```

> [!TIP]
> Get your API key from **Settings** → **Developer** in the dashboard. Click **Generate New Key** if you don't have one.

---

## Quick Start

Send your first WhatsApp message in seconds:

:::code-group

```curl
curl -X POST "{{API_URL}}/v1/messages/send" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [{
      "clientWaNumber": "919876543210",
      "messageType": "text",
      "message": "Hello from the API!"
    }]
  }'
```

```javascript
const response = await fetch('{{API_URL}}/v1/messages/send', {
  method: 'POST',
  headers: {
    Authorization: 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    messages: [
      {
        clientWaNumber: '919876543210',
        messageType: 'text',
        message: 'Hello from the API!',
      },
    ],
  }),
});

const data = await response.json();
console.log(data);
```

```python
import requests

response = requests.post(
    '{{API_URL}}/v1/messages/send',
    headers={
        'Authorization': 'Bearer YOUR_API_KEY',
        'Content-Type': 'application/json'
    },
    json={
        'messages': [{
            'clientWaNumber': '919876543210',
            'messageType': 'text',
            'message': 'Hello from the API!'
        }]
    }
)

print(response.json())
```

:::

---

## API Sections

:::cards
[Messages](/docs/api/messages) - Send text, media, templates, and interactive messages
[Contacts](/docs/api/contacts) - Create, update, and manage WhatsApp contacts
[Groups](/docs/api/groups) - Create WhatsApp groups and fetch active groups directly from Meta
[Templates](/docs/api/templates) - Create and manage message templates
[Campaigns](/docs/api/campaigns) - Send bulk messaging campaigns
[Calls](/docs/api/calls) - Place WhatsApp / SIP / AI-agent outbound voice calls
[Webhooks](/docs/api/webhooks) - Receive real-time event notifications
[Authentication](/docs/api/authentication) - API key setup and usage
[Code Editor](/docs/api/code-editor) - Execute custom functions from the code editor
[Schedule](/docs/api/schedule) - Schedule messages, campaigns, and nudges
[Chatbot](/docs/api/chatbot) - Activate, trigger, and test chatbot conversations
:::

---

## Response Format

All API responses follow a consistent format.

### Success Response

```json
{
  "code": "OK",
  "message": "Success",
  "data": {
    // Response data here
  }
}
```

### Error Response

```json
{
  "code": "BAD_REQUEST",
  "errorMessage": "Validation failed: clientWaNumber is required"
}
```

---

## HTTP Status Codes

| Code | Status                | Description                                |
| ---- | --------------------- | ------------------------------------------ |
| 200  | OK                    | Request successful                         |
| 400  | Bad Request           | Invalid request parameters or body         |
| 401  | Unauthorized          | Missing or invalid API key                 |
| 403  | Forbidden             | Valid API key but insufficient permissions |
| 404  | Not Found             | Resource doesn't exist                     |
| 429  | Too Many Requests     | Rate limit exceeded                        |
| 500  | Internal Server Error | Server-side error                          |

---

## Phone Number Format

> [!IMPORTANT]
> Always use full international format **without** the + prefix.

| Format                  | Example         | Valid |
| ----------------------- | --------------- | ----- |
| With country code, no + | `919876543210`  | ✅    |
| USA format              | `14155551234`   | ✅    |
| With + prefix           | `+919876543210` | ❌    |
| Without country code    | `9876543210`    | ❌    |

---

## Rate Limits

API requests are rate limited per business account. If you exceed the limit, you'll receive a `429` response.

**Best Practices:**

- Implement exponential backoff for retries
- Batch messages where possible (up to 1000 per request)
- Use webhooks instead of polling for status updates
