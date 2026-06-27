---
title: MoEngage
description: Send WhatsApp messages from MoEngage campaigns and receive delivery status + replies back
icon: Send
order: 3
---

# MoEngage Integration

Configure the platform as a WhatsApp connector in MoEngage so its campaigns, journeys, and transactional flows deliver WhatsApp messages through your account — and pipe delivery receipts and customer replies back to MoEngage.

> [!NOTE]
> Estimated setup time: ~10 minutes. You'll need MoEngage admin access and an API key from the platform. Adding the connector in MoEngage may require help from your MoEngage account team.

---

## Before you start

- An active WhatsApp Business number connected to the platform
- A platform API key (**Settings → Developer Tools → API Keys**)
- MoEngage admin access to the workspace you want to wire up

---

## Step-by-step

### 1. Open the Sender configuration tab

Log in to the MoEngage dashboard and navigate to **Settings → Channels → WhatsApp → Sender configuration**.

### 2. Add the connector

Click **+ Add connector**, then pick the connector for the platform from the **Choose a connector** dropdown and add it.

### 3. Add a sender

Select the connector you just added. **Sender 1** is auto-populated — use **+ Sender** if you need additional senders. Enter the **Sender name** and your **WhatsApp Business Number** (with country code).

### 4. Set the authentication (Auth key)

In the sender details, enter your platform API key in the **Auth key** (API key) field.

> [!IMPORTANT]
> MoEngage sends this credential using the `Authentication` header (not the standard `Authorization`). The platform accepts both, so it works either way — just make sure the value resolves to `Bearer <YOUR_API_KEY>`. If MoEngage doesn't add the `Bearer ` prefix for you, enter the full value `Bearer <YOUR_API_KEY>` in the field.

> [!WARNING]
> Treat your API key like a password. Don't paste it into shared docs or screenshots — rotate it from **Settings → Developer Tools** if you suspect it has leaked.

### 5. Set the Integration URL

If the connector asks for the request / endpoint URL, use:

- **Request Type**: `POST`
- **Endpoint URL**: `{{API_URL}}/v1/integrations/mo-engage`

MoEngage POSTs each message here using Meta's WhatsApp template format — see [Supported message types](#supported-message-types) below.

### 6. Copy the delivery report callback URL

In the same sender details, copy the **delivery-report-callback-url** that MoEngage shows for this sender (no separate webhook page). You'll paste it into the platform in the next step.

### 7. Save, then wire up the callback in the platform

Click **Save** in MoEngage. Then, in the platform, open **Settings → Webhook Manager**, edit the **MoEngage** field, and paste the callback URL from Step 6. The platform will forward delivery status and customer replies to MoEngage from then on.

> [!TIP]
> The **MoEngage** webhook field carries both delivery status (`sent` / `delivered` / `read` / `failed`) and inbound replies (text + quick-reply button clicks) back to MoEngage.

> [!NOTE]
> The sender connector is the platform-specific part. To actually send, MoEngage also needs its own channel setup completed — adding your approved templates (**Approved templates** tab) and configuring WhatsApp opt-in (**General settings** tab). Follow MoEngage's WhatsApp configuration guide for those.

---

## How it works

```
MoEngage campaign ──POST──▶ {{API_URL}}/v1/integrations/mo-engage ──▶ WhatsApp send
                                                                        │
                                                                        ▼
                                            Status + replies ──▶ MoEngage webhook ──▶ MoEngage
```

- MoEngage POSTs to the integration endpoint with the template details and personalization variables, plus a unique `msg_id`.
- The platform sends the WhatsApp message and tracks it against that `msg_id`.
- As Meta returns `sent` / `delivered` / `read` / `failed` — and when the customer replies — the platform forwards each event to the callback URL you pasted in Step 7, tagged with the original `msg_id`.

---

## Supported message types

| Type               | Supported variables                                                       |
| ------------------ | ------------------------------------------------------------------------- |
| Standard template  | Header (image / video / document), body (`text`, `currency`, `date_time`) |
| Buttons            | Quick reply, URL (dynamic), copy code (coupon)                            |
| Limited-time offer | Offer expiry + copy-code / URL buttons                                    |

Each message must include a unique `msg_id`, the recipient `to` (E.164 format), and a `template` with its `name`, `language.code`, and `components`.

---

## Webhook field reference

| Field      | Description                                                           |
| ---------- | --------------------------------------------------------------------- |
| `moEngage` | Delivery status events and inbound replies forwarded back to MoEngage |

See [Webhooks](/docs/api/webhooks) for the full payload format.

---

## Troubleshooting

> [!WARNING]
> Seeing `401 Unauthorized`? Check the auth header. The value must be your API key prefixed with `Bearer ` (with a space) — e.g. `Bearer hk_live_xxx`. MoEngage uses the header key `Authentication`; `Authorization` works too.

> [!INFO]
> Status or replies not reaching MoEngage? Open **Settings → Webhook Manager**, confirm the **MoEngage** field is enabled with the callback URL from Step 7, and check the recent delivery logs.

:::support
Stuck on a step? Reach out via the **Contact Support** link in the sidebar — share the configuration name and a screenshot of the failing step and we'll unblock you fast.
:::
