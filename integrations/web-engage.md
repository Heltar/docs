---
title: WebEngage
description: Send WhatsApp messages from WebEngage journeys and receive delivery status back
icon: Send
order: 1
---

# WebEngage Integration

Configure WebEngage as a Private WSP so its journeys, campaigns, and transactional flows can deliver WhatsApp messages through your account, and pipe delivery + read receipts back into your webhooks.

> [!NOTE]
> Estimated setup time: ~5 minutes. You'll need WebEngage admin access and an API key from the platform.

---

## Before you start

- An active WhatsApp Business number connected to the platform
- A platform API key (Settings -> Developer Tools -> API Keys)
- WebEngage admin access to the workspace you want to wire up

---

## Step-by-step

### 1. Open WebEngage Integrations

Log in to WebEngage and click **Integrations** in the top right corner.

![WebEngage integrations panel](/integration-images/web-engage/Integrations.webp)

### 2. Find WhatsApp Setup

Scroll to **WhatsApp Setup** and click **Configure**.

![WhatsApp configure card](/integration-images/web-engage/whatsapp-configure.webp)

### 3. Add a WhatsApp Service Provider

Click **Add WhatsApp Service Provider**.

![Add WSP button](/integration-images/web-engage/WSP.webp)

### 4. Choose Private WSP

Pick **Private WSP** and enter the configuration name as `Heltar`.

![Private WSP selection](/integration-images/web-engage/privateWSP.webp)

### 5. Enter your WhatsApp number

Enter your WhatsApp business number including the country code.

![WhatsApp number field](/integration-images/web-engage/whatsapp_number.webp)

### 6. Set the Integration URL

Use the values below in WebEngage's request configuration:

- **Request Type**: `POST`
- **HTTP Endpoint**: `{{API_URL}}/v1/integrations/web-engage`

![Integration URL configuration](/integration-images/web-engage/integration_url.webp)

### 7. Send personalisation variables

Choose **Request Type** as `Send personalisation variables`.

![Send personalisation toggle](/integration-images/web-engage/sendPersonalisation.webp)

### 8. Add the Authorization header

Add a custom header with key `Authorization` and value `Bearer <YOUR_API_KEY>`.

![Authorization header](/integration-images/web-engage/header.webp)

> [!WARNING]
> Treat your API key like a password. Don't paste it into shared docs or screenshots — rotate it from Settings -> Developer Tools if you suspect it has leaked.

### 9. Save the configuration

Click **Save**.

![Save button](/integration-images/web-engage/save.webp)

### 10. Open the Webhook URL panel

Click **View Webhook URL**.

![View webhook URL link](/integration-images/web-engage/view_webhook.webp)

### 11. Copy the WebEngage webhook URL

Copy the webhook URL — you'll paste it into the platform on the next step.

![Copy webhook URL](/integration-images/web-engage/copy_webhook_url.webp)

### 12. Paste it back into the platform

In the platform, open **Integrations -> WebEngage** and paste the webhook URL into the **Delivery Report Callback URL** field, then click **Submit**. The platform will start forwarding `webEngageStatus` events to that URL.

> [!TIP]
> You can also configure this from **Settings -> Webhook Manager** by editing the `webEngageStatus` field directly.

---

## How it works

```
WebEngage journey ──POST──▶ {{API_URL}}/v1/integrations/web-engage ──▶ WhatsApp send
                                                                          │
                                                                          ▼
                                                       Status event ──▶ webEngageStatus webhook ──▶ WebEngage
```

- WebEngage POSTs to the integration endpoint with the personalisation variables and template details.
- The platform sends the WhatsApp message and records the `wamid`.
- As Meta returns `sent` / `delivered` / `read` / `failed`, the platform forwards each event to the webhook URL you pasted in Step 12.

---

## Webhook field reference

| Field             | Description                                        |
| ----------------- | -------------------------------------------------- |
| `webEngageStatus` | Delivery status events forwarded back to WebEngage |

See [Webhooks](/docs/api/webhooks) for the full payload format.

---

## Troubleshooting

> [!WARNING]
> If WebEngage shows `401 Unauthorized`, double-check the `Authorization: Bearer ...` header — the value must be the API key prefixed with `Bearer ` (with a space).

> [!INFO]
> Status events not reaching WebEngage? Open **Settings -> Webhook Manager**, confirm `webEngageStatus` is enabled, and inspect the recent delivery logs.

:::support
Stuck on a step? Reach out via the **Contact Support** link in the sidebar — share the configuration name and a screenshot of the failing step and we'll unblock you fast.
:::
