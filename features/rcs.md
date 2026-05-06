---
title: RCS Messaging
description: Send and receive Rich Communication Services (RCS) messages over Route Mobile + Jio alongside WhatsApp
icon: MessageSquare
order: 11
---

# RCS Messaging

Add **Rich Communication Services (RCS)** as a parallel channel to WhatsApp on the same Heltar account. Customers reachable on RCS show up as their own conversation thread in the inbox — distinct from any WhatsApp conversation with the same phone number — so agents always know which channel they're replying on.

Heltar uses **Route Mobile (RML)** as the RCS gateway. You provide the bot/agent name and authentication token Route Mobile gave you, and your existing send and receive flows (sync API, campaigns, templates, chatbots) start working for RCS automatically.

> [!NOTE]
> Estimated setup time: ~10 minutes. You'll need an RCS bot already provisioned on Route Mobile (with a `bot_name` and a JWT auth token).

---

## Quick start (TL;DR)

Three steps. **No new APIs to learn — your existing send code keeps working.**

### Step 1 — Add the Route Mobile API key (once per business)

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
    "metadata": { "type": "route_mobile_rcs", "botName": "rml_jbm" }
  }'
```

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

The platform routes the call to Route Mobile, the message lands on the customer's RCS-capable handset, and shows up in the inbox under a separate RCS thread (with a green **RCS** badge).

### Step 3 — Point Route Mobile's callbacks at our webhook

In your Route Mobile dashboard, set the webhook URL to:

```
POST https://api.heltar.com/v1/webhooks/rcs
```

This is a **single global URL** — no per-business setup. Inbound replies, delivery receipts, chip clicks, and media uploads all flow through this URL automatically.

> [!TIP]
> If you ever need to swap or rotate the Route Mobile token, just re-POST to `/v1/api-keys` with the same `botName` — Heltar updates the key in place. No need to delete first.

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

Heltar renders your approved template body (substituting `{{1}}`, `{{2}}`, …) and translates the buttons into the format Route Mobile expects:

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

Same `messageType: 'location'` shape you already use for WhatsApp. Route Mobile has no standalone outbound location payload, so Heltar emits a text message describing the place plus a single tappable map chip — the customer taps the chip to open the spot in their map app.

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

Once your Route Mobile dashboard is pointed at `POST https://api.heltar.com/v1/webhooks/rcs`, the following events are picked up automatically:

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
- **`botName` is globally unique** across all Heltar businesses. Trying to register a `botName` that another business already owns returns `409 Conflict` (matches the reality that Route Mobile won't let two clients share a bot anyway).
- **Carousel intro text isn't surfaced** — Route Mobile's carousel format doesn't accept a body text alongside the cards. Send a separate text message before the carousel if you need an intro.

---

## Troubleshooting

**`409 Conflict — RCS bot name 'X' is already registered to another business`**
Another Heltar tenant has already saved an active key with the same `botName`. Confirm with Route Mobile that the bot really belongs to your account and ask the other tenant to remove their key.

**`No active Route Mobile RCS API key found for the business`**
The send call ran before any key was saved. Re-check `GET /v1/api-keys` for an entry with `"type": "route_mobile_rcs"` and `"isActive": true`.

**`text` field empty error from Route Mobile**
The rendered template body came out empty (template's approved body text is blank, or all variables resolve to empty strings). Check the template content.

**Webhook arrives but nothing shows up in the inbox**
Most common cause: the `botName` registered with Route Mobile doesn't match the `botName` you saved in `metadata`. Re-check both sides.
