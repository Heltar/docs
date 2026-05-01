---
title: WhatsApp Groups
description: Send and receive messages in small WhatsApp groups from your business number
icon: Users
order: 11
---

# WhatsApp Groups

Heltar now supports WhatsApp Cloud API's **Groups** feature — create small groups from your business number, receive participant messages in your Inbox, and send text, media, and template replies into the group.

Groups look and feel just like any other chat in the Inbox, with a Users icon badge to tell them apart at a glance.

> [!TIP]
> Groups are **small-team chats**, not broadcast communities. Meta limits groups to **8 participants** total and bans calling inside them. If you need one-to-many reach, use [Campaigns](/docs/features/campaigns).

---

## Prerequisites

- Your WhatsApp number must be registered as an **Official Business Account (OBA)**. Groups are not available on Coexistence or Multi-solution Conversations.
- Groups are visible to a **single** Cloud API business per group — your number and someone else's business number can't both be in the same group.

---

## Creating a group

Use the Groups page or the API to create a group. Meta accepts the request and returns a `request_id`; the actual `group_id` arrives moments later via a webhook, and the group shows up in your Inbox shortly after.

See the [Groups API reference](/docs/api/groups) for the full payload.

**Join approval modes:**

- `auto_approve` — anyone with the invite link joins immediately.
- `approval_required` — admins must approve each join request (managed through the Groups API today; UI controls are on the roadmap).

---

## Inbox experience

Once a group exists, it appears in the Inbox chat list like any other conversation, with two visual differences:

- **Users icon** next to the group name (brand-green).
- **No Call button** — Meta's Calling API isn't available in groups, so the UI hides it.

Inside the chat you can:

- Send **text**, **images**, **videos**, **audio**, **documents** — all native media types.
- Send **text templates** and **media templates** — useful for structured announcements. Use dedicated group templates (see below).
- See per-member sender identity preserved on each incoming message (shown in the UI once you've enabled group chats).

You **cannot** send (Meta blocks these in groups):

- Interactive messages (buttons, list pickers, flows)
- Authentication templates (OTP)
- Location / contact cards
- Commerce / catalog / product messages
- Voice or video calls

The composer disables these options automatically when the active chat is a group.

---

## Templates in groups

Meta recommends creating **dedicated templates** for group messaging rather than reusing your 1:1 templates. Two reasons:

1. Template **performance metrics are not collected** for messages sent to groups — reused templates will lose their stats attribution.
2. Group-appropriate copy usually differs from 1:1 copy (no personalization of the recipient's name, etc.).

Mark templates as "group" in your naming convention (`welcome_group_en`, `order_update_group_en`) to keep them separate.

---

## How messages are stored

Under the hood, a group is stored in the same contacts table as an individual — the group's Meta ID occupies the `clientWaNumber` field, and the group's subject is its display name. This is a **zero-migration** design: if you're an existing Heltar user, groups just start appearing in your data without any schema upgrade or reindex.

Per-message sender identity (which member posted what) is saved on the message's metadata blob, so downstream analytics and audit trails still tell you who said what inside the group.

---

## What Heltar handles automatically

The Inbox stays in sync with Meta through four webhook feeds. You don't need to do anything — just subscribe them in your Meta App configuration:

- **`group_lifecycle_update`** — groups appear in your Inbox within seconds of `POST /v1/groups` completing on Meta. Deleted groups vanish automatically.
- **`group_participants_update`** — member roster and join-request list stay current.
- **`group_settings_update`** — subject, description, profile picture updates land in real time.
- **`group_status_update`** — if Meta suspends your group for policy reasons, Heltar marks it blocked; when suspension is lifted, the group comes back automatically.

Each event persists operational state on `meta_data` (not user-defined `attributes`) so your CRM data stays untouched.

## What's on the roadmap

The following Meta API capabilities are wired up on the backend but **not yet surfaced as UI actions** inside the Inbox:

- Participant add / promote / demote actions
- Pin / unpin messages
- Group subject / description editing from the chat info panel

Available today in the group info panel: invite link copy & reset, join-request approve / reject, and participant remove. Ping the Heltar team if any of the roadmap items are blocking your use case.

---

## Frequently asked questions

**Can I start a voice call with a group member from inside the group chat?**
No. Meta forbids calls in group context, even to individual members. Open the member's separate 1:1 chat (if you have one) to call.

**Can chatbots reply in a group?**
No — we intentionally disable chatbot auto-replies in groups. A bot answering every member's message in a shared chat is almost always unwanted. If you need automation inside a group, use templates triggered by your own backend events.

**How is billing calculated for group messages?**
Meta bills per group message (not per member), under their normal template / session-window pricing. See [Meta's Groups pricing docs](https://developers.facebook.com/docs/whatsapp/cloud-api/groups/) for the latest rates.

**Will opt-in / opt-out semantics apply in groups?**
Opt-in/out is a per-individual concept. Heltar treats the group itself as always-in for send purposes; individual members' opt-out choices continue to apply to their 1:1 chats independently.
