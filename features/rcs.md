---
title: RCS Messaging
description: Send and receive Rich Communication Services (RCS) messages over Route Mobile + Jio or directly through Google RCS for Business, alongside WhatsApp
icon: MessageSquare
order: 11
---

# RCS Messaging

Add **Rich Communication Services (RCS)** as a parallel channel to WhatsApp on the same Heltar account. Customers reachable on RCS show up as their own conversation thread in the inbox — distinct from any WhatsApp conversation with the same phone number — so agents always know which channel they're replying on.

Heltar supports two RCS gateways. Pick one per business:

- **Route Mobile (RML)** — you provide the bot name and authentication token Route Mobile gave you.
- **Google RCS for Business (RBM), direct** — you provide your RBM agent's id and a service-account key from the RBM developer console; Heltar calls Google's RBM API for you.

Either way your existing send and receive flows (sync API, campaigns, templates, chatbots) start working for RCS automatically.

> [!NOTE]
> Estimated setup time: ~10 minutes. You'll need an RCS agent already provisioned — on Route Mobile (a `bot_name` and a JWT auth token) or on Google RCS (a launched agent, its agent id, and a service-account JSON key).

---

## Quick start (TL;DR)

Three steps. **No new APIs to learn — your existing send code keeps working.**

### Step 1 — Add the RCS API key (once per business)

**Option A — Route Mobile**

```bash
curl -X POST 'https://api.heltar.com/v1/api-keys' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: <your-Heltar-JWT>' \
  -d '{
    "type": "route_mobile_rcs",
    "task": "rcs",
    "key": "<RouteMobile JWT token>",
    "name": "RouteMobile RCS",
    "isActive": true,
    "metadata": { "botName": "rml_jbm" }
  }'
```

**Option B — Google RCS for Business (direct)**

In the [RBM developer console](https://business-communications.cloud.google.com/console/) open your agent, note its **Agent ID** (e.g. `my_brand_abc123_agent`) and **Hosting region**, and download a **service account key** (JSON). Then:

```bash
curl -X POST 'https://api.heltar.com/v1/api-keys' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: <your-Heltar-JWT>' \
  -d '{
    "type": "google_rcs",
    "task": "rcs",
    "key": "<service-account JSON, serialized as one string>",
    "name": "Google RCS",
    "isActive": true,
    "metadata": {
      "agentId": "my_brand_abc123_agent",
      "region": "europe",
      "clientToken": "<webhook client token from the RBM console>"
    }
  }'
```

- `key` is the whole service-account file as a single JSON string (`jq -Rs . service-account.json` produces exactly that).
- `region` is the agent's hosting region: `europe`, `us`, `asia`, or `global` (default). It selects Google's regional API host.
- `clientToken` is the **Client token** shown next to the webhook URL in the RBM console — save the **same** value here and in the console webhook (Step 3), or Verify fails. Agents served through a partner-level webhook can omit it. Like `key`, it is stored encrypted and never returned by `GET /v1/api-keys`.

Activating a Google RCS key deactivates any Route Mobile RCS key on the same business (and vice versa) — one gateway per business.

### Step 2 — Send messages by appending `@rcs` to the phone number

Same `POST /v1/messages/send` endpoint you already use for WhatsApp — just stamp `@rcs` on the recipient:

```bash
curl -X POST 'https://api.heltar.com/v1/messages/send' \
  -H 'Authorization: <your-Heltar-JWT>' \
  -H 'Content-Type: application/json' \
  -d '{
    "messages": [{
      "clientWaNumber": "919999999999@rcs",
      "messageType": "text",
      "message": "Hi from RCS!"
    }]
  }'
```

The platform routes the call to your gateway, the message lands on the customer's RCS-capable handset, and shows up in the inbox under a separate RCS thread (with a green **RCS** badge).

### Step 3 — Point your gateway's callbacks at our webhook

**Route Mobile** — in the Route Mobile dashboard set the webhook URL to:

```
POST https://api.heltar.com/v1/webhooks/rcs
```

**Google RCS** — set the webhook in the [RCS for Business developer console](https://business-communications.cloud.google.com/console/):

1. Open the console and sign in, then **click your agent** (e.g. Route Mobile Europe).
2. Go to **Integrations** (left nav) → **Webhook** → **Configure**.
3. **Webhook endpoint URL**:
   ```
   https://api.heltar.com/v1/webhooks/rcs/google
   ```
4. **Client token** — the console shows a client token next to the webhook. **The exact same token must also be saved on your Heltar key** as `metadata.clientToken` (Step 1, Option B). If the two don't match, **Verify fails and every later event is dropped**. To change it, update it in both places.
5. Click **Verify**. Google sends a one-time check to the URL and, on success, marks the webhook verified.

> [!IMPORTANT]
> The client token is the shared secret between Google and Heltar. It must be **identical** in the RBM console webhook and in your Heltar key's `metadata.clientToken`. (Agents served by a partner-level webhook use its token via `GOOGLE_RCS_WEBHOOK_CLIENT_TOKENS` instead.)

> A partner account can instead set one **partner-level webhook** that covers every agent under it — only do this if all agents on that account are yours, since it redirects their callbacks too.

Both are **single global URLs** — no per-business setup. Inbound replies, delivery receipts, chip clicks, and media uploads all flow through automatically; the agent name in each event routes it to the right business.

> [!TIP]
> If you ever need to swap or rotate the Route Mobile token or the Google service-account key, just re-POST to `/v1/api-keys` with the same `botName` / `agentId` — Heltar updates the key in place. No need to delete first.

---

## How RCS sits next to WhatsApp

A customer reachable on **both** WhatsApp and RCS shows up as **two threads** in the inbox — same name, same phone, but with the RCS thread carrying a green **RCS** badge so agents can tell them apart at a glance.

You don't have to do anything special to "create" an RCS contact. The first time you send a message to `<phone>@rcs`, the conversation appears automatically. Bulk-importing works the same way: any row in `POST /v1/clients` whose `clientWaNumber` ends with `@rcs` is created as an RCS contact.

---

## Send formats

All examples below use the same `POST /v1/messages/send` endpoint you use for WhatsApp.

### Plain text

```json
{
  "messages": [
    {
      "clientWaNumber": "919999999999@rcs",
      "messageType": "text",
      "message": "Your order has been shipped."
    }
  ]
}
```

### Template (with variables and buttons)

Heltar renders your approved template body (substituting `{{1}}`, `{{2}}`, …) and translates the buttons into the format your gateway expects:

```json
{
  "messages": [
    {
      "clientWaNumber": "919999999999@rcs",
      "messageType": "template",
      "templateName": "order_confirmation",
      "languageCode": "en_US",
      "variables": [
        {
          "type": "body",
          "parameters": [
            { "type": "text", "text": "John" },
            { "type": "text", "text": "#1234" }
          ]
        }
      ]
    }
  ]
}
```

What the customer sees depends on the template shape:

| Template structure                 | RCS rendering                                  |
| ---------------------------------- | ---------------------------------------------- |
| Body only                          | Plain text message                             |
| Text header + body + buttons       | Text message with the header on top            |
| Media header + body + buttons      | Rich card (image / video / pdf + text + chips) |
| Carousel (multiple cards)          | Swipeable card list                            |
| Body + media (image / video / pdf) | Media message with caption                     |

Reply chips (`QUICK_REPLY`), URL buttons, and phone-call buttons all map across automatically. Static buttons (those without `{{N}}` placeholders) come from the approved template definition so they aren't dropped.

### Interactive (reply chips, URL chips, location, calendar)

Use the same `messageType: 'interactive'` shape. RCS supports two extensions on top of WhatsApp's reply chips — **location** and **calendar** chips — accepted on the same payload:

```json
{
  "messageType": "interactive",
  "clientWaNumber": "919999999999@rcs",
  "interactive": {
    "type": "button",
    "body": { "text": "Pick a slot" },
    "action": {
      "buttons": [
        { "type": "reply", "reply": { "id": "yes", "title": "Yes" } },
        {
          "type": "calendar",
          "calendar": {
            "title": "Demo call",
            "start": "2026-05-20T10:00:00Z",
            "end": "2026-05-20T10:30:00Z"
          }
        }
      ]
    }
  }
}
```

Sending the same payload to a regular phone (no `@rcs`) routes through WhatsApp instead — WhatsApp rejects the calendar chip but accepts the reply chip. Both cases work without any code branches on your side.

### Media

```json
{
  "messages": [
    {
      "clientWaNumber": "919999999999@rcs",
      "messageType": "media",
      "mediaType": "image",
      "url": "https://your-cdn.com/banner.jpg",
      "caption": "Diwali offer"
    }
  ]
}
```

Image, video, and PDF are all supported.

### Location

Same `messageType: 'location'` shape you already use for WhatsApp. Neither gateway has a standalone outbound location payload, so Heltar emits a text message describing the place plus a single tappable map chip — the customer taps the chip to open the spot in their map app.

```json
{
  "messages": [
    {
      "clientWaNumber": "919999999999@rcs",
      "messageType": "location",
      "location": {
        "latitude": 25.688562926103575,
        "longitude": 85.20774517016658,
        "name": "Heltar HQ",
        "address": "Patna, Bihar"
      }
    }
  ]
}
```

`name` and `address` are optional. When both are absent, the message body falls back to the raw `latitude, longitude` string and the chip label reads `View on map`.

---

## What flows in through the webhook

Once your gateway is pointed at the webhook URL from Step 3, the following events are picked up automatically:

| Event                                        | Result in your inbox                                                                 |
| -------------------------------------------- | ------------------------------------------------------------------------------------ |
| Inbound text                                 | Saved as a text message; chatbots run; real-time inbox update                        |
| Inbound media (image / video / pdf)          | Stored on Heltar's CDN with a stable link — the inbox renders it like any media      |
| Inbound location                             | Saved as a location message with a map link                                          |
| Suggested-action click (URL / dial)          | Saved as a button reply so chatbots can match on the postback                        |
| Suggested-reply click                        | Same as above                                                                        |
| Status: sent / delivered / read              | Updates the message ticks                                                            |
| Status: failed                               | Marks the message as failed; the failure reason is captured in the message details   |
| Fallback to WhatsApp                         | Treated as a failed RCS attempt (Route Mobile delivered the fallback channel's copy) |
| Google RCS: delivered / read events          | Updates the message ticks (Google sends no separate `sent` receipt or async failure) |
| Typing indicator and other ephemeral signals | Ignored                                                                              |

---

## What agents see in the inbox

- **Chat list** — RCS contacts show a small green **RCS** badge next to the timestamp.
- **Chat header** — same badge next to the contact's name, with the subtitle reading `RCS · Contact Details` instead of `Contact Details`.
- **Message bubbles** — render identically to WhatsApp (text, media, replies, chips).

Inbound chip clicks come back as button replies, so chatbot rules that match on `button_reply.id` work for both channels without changes.

---

## Limits and current trade-offs

- **One active RCS key per business** — re-submitting the API call replaces the previous key transparently.
- **`botName` / `agentId` is globally unique** across all Heltar businesses. Trying to register an agent name that another business already owns returns `409 Conflict` (matches the reality that a gateway won't let two clients share an agent anyway).
- **Carousel intro text isn't surfaced** — neither gateway's carousel format accepts a body text alongside the cards. Send a separate text message before the carousel if you need an intro.
- **Google RCS shapes** — a carousel needs 2–10 cards (a single card is sent as a standalone rich card); media with a caption is sent as a rich card (Google has no captioned file message); at most 11 chips per message and 4 per card, chip labels are cut at 25 characters.
- **Google RCS traffic type** — templates are sent as `PROMOTION` (MARKETING), `AUTHENTICATION`, or `TRANSACTION` (UTILITY) based on the approved template category; all session messages go as `TRANSACTION`.

---

## Troubleshooting

**`409 Conflict — RCS bot name 'X' is already registered to another business`**
Another Heltar tenant has already saved an active key with the same `botName`. Confirm with Route Mobile that the bot really belongs to your account and ask the other tenant to remove their key.

**`No active Route Mobile RCS API key found for the business`** / **`No active Google RCS API key found for the business`**
The send call ran before any key was saved. Re-check `GET /v1/api-keys` for an entry with `"task": "rcs"` and `"isActive": true`.

**RBM console says webhook verification failed**
The client token in the console doesn't match the one Heltar knows. Save it as `metadata.clientToken` on your `google_rcs` key (or ask Heltar to register it for a partner-level webhook), then click **Verify** again.

**Google returns `404` on send**
The recipient isn't RCS-enabled on a Google-connected carrier. Fall back to WhatsApp or SMS for that number.

**`text` field empty error from Route Mobile**
The rendered template body came out empty (template's approved body text is blank, or all variables resolve to empty strings). Check the template content.

**Webhook arrives but nothing shows up in the inbox**
Most common cause: the agent name registered with the gateway doesn't match what you saved in `metadata` (`botName` for Route Mobile, `agentId` for Google — without the `@rbm.goog` suffix). For Google, also confirm the webhook's client token matches `metadata.clientToken`, since unsigned events are dropped.
