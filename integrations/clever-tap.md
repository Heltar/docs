---
title: CleverTap
description: Send WhatsApp messages from CleverTap campaigns and import templates through the platform
icon: Send
order: 2
---

# CleverTap Integration

Connect CleverTap to the platform as a **Generic** WhatsApp provider so its campaigns can deliver WhatsApp messages through your account, sync delivery + inbound events back, and import your existing templates automatically.

> [!NOTE]
> Estimated setup time: ~5 minutes. You'll need CleverTap account access and a platform API key for the Authorization header.

---

## Before you start

- An active WhatsApp Business number connected to the platform
- A platform API key (Settings -> Developer Tools -> API Keys)
- CleverTap account access for the workspace you want to wire up

---

## 1. Navigate to WhatsApp Settings

Open CleverTap and walk through to the WhatsApp provider configuration screen.

### 1. Open the CleverTap Demo workspace

Login to CleverTap and click **Explore Demo** on the top right corner.

![CleverTap login](/integration-images/clever-tap/step1.webp)

### 2. Pick the E-commerce demo

Click on the **E-commerce** option.

![Select E-commerce](/integration-images/clever-tap/step2.webp)

### 3. Open Settings

Select **Settings** from the left menu.

![Settings menu](/integration-images/clever-tap/step3.webp)

### 4. Open the WhatsApp channel

Select **Channels** and then choose **WhatsApp**.

![Channels -> WhatsApp](/integration-images/clever-tap/step4.webp)

### 5. Switch to WhatsApp Connect

Select the **WhatsApp Connect** tab on the top bar.

![WhatsApp Connect tab](/integration-images/clever-tap/step5.webp)

### 6. Open Provider Configuration

Click on the **Provider Configuration** button.

![Provider Configuration button](/integration-images/clever-tap/step6.webp)

### 7. Choose the Generic provider

Choose **Generic** in the provider options.

![Generic provider](/integration-images/clever-tap/step7.webp)

---

## 2. Configure Webhooks

These callback URLs let CleverTap receive real-time delivery status updates and inbound customer replies from the platform.

In the platform, open **App Store -> Integrations -> CleverTap** (or **Settings -> Webhook Manager**) and enter:

- **Delivery Report Callback URL** -> the URL CleverTap exposes for delivery receipts. Saved on the platform as the `cleverTapStatus` webhook field.
- **Inbound Message Callback URL** -> the URL CleverTap exposes for incoming messages. Saved on the platform as the `cleverTapMessages` webhook field.

Click **Save Webhooks**.

> [!TIP]
> You can find both URLs inside CleverTap's Generic provider configuration screen, under the delivery and inbound callback fields.

---

## 3. Set Up the Messaging Endpoint

This is the endpoint CleverTap calls when sending WhatsApp messages through the platform.

### 8. Paste the HTTP Endpoint

In CleverTap's Generic provider configuration, set:

- **Method**: `POST`
- **HTTP Endpoint**: `{{API_URL}}/v1/integrations/clever-tap`

You can copy the exact URL from the in-app integration page (**App Store -> Integrations -> CleverTap**).

### 9. Add the Authorization header

Click on **Headers** and enter:

| Key             | Value                   |
| --------------- | ----------------------- |
| `Authorization` | `Bearer <YOUR_API_KEY>` |

![Authorization header](/integration-images/clever-tap/step10.webp)

> [!WARNING]
> Treat your API key like a password. Don't paste it into shared docs or screenshots. Rotate it from Settings -> Developer Tools if you suspect it has leaked.

---

## 4. Import Templates

CleverTap can pull your approved WhatsApp templates directly from the platform so you don't have to recreate them.

### 10. Configure the Import Templates URL

Scroll to the **Import Templates** section in CleverTap's Provider Configuration and set:

- **Request Type**: `GET`
- **Import Templates URL**: `{{API_URL}}/v1/integrations/clever-tap/templates`

CleverTap will automatically fetch and sync all your approved WhatsApp templates using this endpoint, with pagination support.

---

## How it works

```
CleverTap campaign ──POST──▶ {{API_URL}}/v1/integrations/clever-tap ──▶ WhatsApp send
                                                                          │
                                                                          ▼
                                                       Status event ──▶ cleverTapStatus webhook ──▶ CleverTap
                                                                          │
                                                       Inbound reply ──▶ cleverTapMessages webhook ──▶ CleverTap

CleverTap template sync ──GET──▶ {{API_URL}}/v1/integrations/clever-tap/templates ──▶ Templates imported
```

- CleverTap POSTs to the messaging endpoint with template details and personalisation parameters.
- The platform sends the WhatsApp message and records the `wamid` against your CleverTap `msgId`.
- As Meta returns `sent` / `delivered` / `read` / `failed`, the platform forwards each event to the `cleverTapStatus` URL.
- Inbound user replies (text, media, button taps, location) are forwarded to the `cleverTapMessages` URL.
- Template sync uses a paginated GET endpoint that returns templates in Meta format.

---

## Webhook field reference

| Field               | Description                                        |
| ------------------- | -------------------------------------------------- |
| `cleverTapStatus`   | Delivery status events forwarded back to CleverTap |
| `cleverTapMessages` | Inbound user messages forwarded back to CleverTap  |

See [Webhooks](/docs/api/webhooks) for the full payload format.

---

## Troubleshooting

> [!WARNING]
> If CleverTap shows `401 Unauthorized`, double-check the `Authorization: Bearer ...` header. The value must be the API key prefixed with `Bearer ` (with a space).

> [!INFO]
> Status events not reaching CleverTap? Open **Settings -> Webhook Manager**, confirm `cleverTapStatus` is enabled, and inspect the recent delivery logs.

> [!INFO]
> Templates not importing? Make sure the request type is `GET` (not `POST`) and that the URL ends in `/v1/integrations/clever-tap/templates`. The endpoint returns templates in Meta format with pagination.

:::support
Stuck on a step? Reach out via the **Contact Support** link in the sidebar. Share a screenshot of the failing step and we'll unblock you fast.
:::
