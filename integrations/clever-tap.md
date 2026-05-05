---
title: CleverTap
description: Send WhatsApp messages from CleverTap campaigns and receive delivery status back
icon: Send
order: 2
---

# CleverTap Integration

Configure CleverTap with the platform as a Private WhatsApp provider so its campaigns, journeys, and transactional flows can deliver WhatsApp messages through your account, and pipe delivery + read receipts back into your webhooks.

> [!NOTE]
> Estimated setup time: ~5 minutes. You'll need CleverTap admin access and an API key from the platform.

---

## Before you start

- An active WhatsApp Business number connected to the platform
- A platform API key (Settings -> Developer Tools -> API Keys)
- CleverTap admin access to the account/region you want to wire up

---

## Step-by-step

### 1. Open CleverTap Channel Settings

Log in to CleverTap and go to **Settings -> Channels -> WhatsApp**.

![CleverTap channels panel](/integration-images/clever-tap/channels.webp)

### 2. Add a WhatsApp Provider

Click **Add Provider** (or **Configure** if you're editing an existing one).

![Add WhatsApp provider](/integration-images/clever-tap/add-provider.webp)

### 3. Choose Custom / Private Provider

Pick **Custom** (also labelled **Private API**) and enter the configuration name as `Heltar`.

![Custom provider selection](/integration-images/clever-tap/custom-provider.webp)

### 4. Enter your WhatsApp number

Enter your WhatsApp business number including the country code. This must match the number connected to the platform.

![WhatsApp number field](/integration-images/clever-tap/whatsapp-number.webp)

### 5. Set the Send API URL

Use the values below in CleverTap's request configuration:

- **Request Method**: `POST`
- **Send API URL**: `{{API_URL}}/v1/integrations/clever-tap`
- **Content-Type**: `application/json`

![Send API URL configuration](/integration-images/clever-tap/send-api-url.webp)

### 6. Add the Authorization header

Add a custom header with key `Authorization` and value `Bearer <YOUR_API_KEY>`.

![Authorization header](/integration-images/clever-tap/header.webp)

> [!WARNING]
> Treat your API key like a password. Don't paste it into shared docs or screenshots — rotate it from Settings -> Developer Tools if you suspect it has leaked.

### 7. Save the configuration

Click **Save**.

![Save button](/integration-images/clever-tap/save.webp)

### 8. Open the Status Callback panel

After saving, CleverTap exposes a **Status Callback URL** (sometimes labelled **Delivery Receipt URL** or **DLR Webhook**) for the provider you just created.

![View status callback URL](/integration-images/clever-tap/view-callback.webp)

### 9. Copy the CleverTap callback URL

Copy the callback URL — you'll paste it into the platform on the next step.

![Copy callback URL](/integration-images/clever-tap/copy-callback.webp)

### 10. Paste it back into the platform

In the platform, open **Integrations -> CleverTap** and paste the callback URL into the **Delivery Report Callback URL** field, then click **Submit**. The platform will start forwarding `cleverTapStatus` events to that URL.

> [!TIP]
> You can also configure this from **Settings -> Webhook Manager** by editing the `cleverTapStatus` field directly. If you also want inbound user replies forwarded to CleverTap, set the `cleverTapMessages` field to the same (or a sibling) URL.

---

## How it works

```
CleverTap campaign ──POST──▶ {{API_URL}}/v1/integrations/clever-tap ──▶ WhatsApp send
                                                                          │
                                                                          ▼
                                                       Status event ──▶ cleverTapStatus webhook ──▶ CleverTap
                                                                          │
                                                       Inbound reply ──▶ cleverTapMessages webhook ──▶ CleverTap
```

- CleverTap POSTs to the integration endpoint with the template details and personalisation parameters.
- The platform sends the WhatsApp message and records the `wamid` against your CleverTap `msgId`.
- As Meta returns `sent` / `delivered` / `read` / `failed`, the platform forwards each event to the `cleverTapStatus` URL you pasted in Step 10.
- Inbound user replies (text, media, button taps, location) are forwarded to the `cleverTapMessages` URL if configured.

---

## Webhook field reference

| Field               | Description                                        |
| ------------------- | -------------------------------------------------- |
| `cleverTapStatus`   | Delivery status events forwarded back to CleverTap |
| `cleverTapMessages` | Inbound user messages forwarded back to CleverTap  |

See [Webhooks](/docs/api/webhooks) for the full payload format.

---

## Importing existing CleverTap templates

If you already have approved WhatsApp templates in CleverTap, you can pull them into the platform from the **App Store -> Bulk Import Templates -> CleverTap** flow instead of recreating each one by hand. Both **Direct** and **Connect** template formats are supported.

---

## Troubleshooting

> [!WARNING]
> If CleverTap shows `401 Unauthorized`, double-check the `Authorization: Bearer ...` header — the value must be the API key prefixed with `Bearer ` (with a space).

> [!INFO]
> Status events not reaching CleverTap? Open **Settings -> Webhook Manager**, confirm `cleverTapStatus` is enabled, and inspect the recent delivery logs.

> [!INFO]
> Messages rejected with `Invalid wabaNumber`? The number CleverTap sends in the payload must match exactly the WhatsApp Business number connected to your platform business — including the country code, with no `+` or spaces.

:::support
Stuck on a step? Reach out via the **Contact Support** link in the sidebar — share the configuration name and a screenshot of the failing step and we'll unblock you fast.
:::
