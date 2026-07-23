---
title: WhatsApp Coexistence
description: Use the WhatsApp Business app and the API on the same number
icon: Smartphone
order: 9
---

# WhatsApp Coexistence

Coexistence connects a number that is already running on the **WhatsApp Business app** to the WhatsApp Cloud API — without giving up the phone app. Your team keeps chatting from the phone exactly as before, while the same number also works on the platform: shared inbox, campaigns, templates, chatbots and the API.

Your existing chats and contacts can be synced in, and every message sent from the phone shows up in the inbox in real time.

**Navigation**: **Settings → WhatsApp Details → Register WhatsApp API**

---

## Eligibility and requirements

Before you start, make sure all of the following are true:

| Requirement           | Details                                                                                            |
| --------------------- | -------------------------------------------------------------------------------------------------- |
| **Active number**     | The number has been active on the WhatsApp **Business** app (not the consumer app) for **7+ days** |
| **App up to date**    | The latest version of the WhatsApp Business app is installed on the phone                          |
| **Phone with camera** | Setup requires scanning a QR code from the phone during the Meta signup flow                       |
| **Facebook access**   | You can log in to the Facebook account that manages (or will manage) your business portfolio       |
| **History sync**      | Optional — you can allow chat history sync to import roughly the **last 6 months** of chats        |

Also good to know before you commit:

- **Groups are not available** on Coexistence numbers.
- **Regional availability is controlled by Meta.** If coexistence is not offered for your country yet, the option will not complete inside Meta's signup popup.
- Messaging throughput on a coexistence number is capped by Meta at **20 messages per second** (a fixed dual-platform limit).

---

## How to connect — step by step

1. Go to **Settings → WhatsApp Details → Register WhatsApp API** and open the signup tab you were directed to by your onboarding contact.
2. Under **Connect your WhatsApp number**, choose the second option:
   **"Existing WhatsApp Business app number"** — _keep chatting from the WhatsApp Business app on your phone while using the platform on the same number_.
3. Review the **Before you connect** checklist that appears (the requirements above).
4. Click **Login with Facebook**. Meta's embedded signup popup opens.
5. Inside the popup: log in, pick (or create) your business portfolio, and follow the **WhatsApp Business app** path.
6. When prompted, open WhatsApp Business on your phone and **scan the QR code** (Settings → Linked devices → Link a device on most phones).
7. On the phone, approve the connection — and choose whether to **share chat history and contacts**. This is your call:
   - **Allow** — the last ~6 months of conversations and your phone contacts are imported in the background.
   - **Decline** — only conversations from this point forward appear.
8. Finish the popup. The number is now connected — the register screen shows a **Coexistence** badge next to your signup type.

> If a warning appears saying the business is already set up, the number was connected before — continue only if you intend to reconnect it.

There is no downtime for the phone app at any point: coexistence numbers are already registered by the app, so the connection attaches the API alongside it rather than re-registering the number.

---

## What syncs in

| Data                       | Behaviour                                                                                                                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Chat history**           | Up to the last ~6 months, imported in background batches after connecting. Both sides of each conversation come in — messages you received and messages you sent from the phone. |
| **Media in history**       | Imported on a best-effort basis. An occasional file that can no longer be fetched keeps its message but without the attachment.                                                  |
| **Contacts**               | Contacts from the phone are mirrored in — new contacts are created and number-only contacts get their saved names. Nothing is ever deleted on the platform side.                 |
| **Ongoing phone messages** | Every message your team sends from the phone app appears in the inbox in real time as a normal outgoing message.                                                                 |

History import runs in phases and can take a while for large accounts. If you declined history sync on the phone, that choice is respected — only new activity syncs.

---

## How the number behaves afterwards

- **Message from either side.** Replies typed on the phone and replies sent from the inbox land in the same conversation, on the same number.
- **Automation stays safe.** Chatbots never react to messages your own team sends from the phone, and phone-sent replies don't create unread counts or notifications — no bot answering your own staff, no false unread badges.
- **Campaigns and templates work normally**, subject to the 20 messages/second coexistence cap.
- **Groups are not available** on coexistence numbers.
- **For API users:** chat-history and phone-echo events are not forwarded to your configured webhooks — webhooks carry live customer traffic only.

---

## Troubleshooting

| Issue                                  | What to do                                                                                                                            |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| QR code expired in the popup           | Close and relaunch the signup — a fresh QR is issued each time.                                                                       |
| Chose **Decline** but want history now | History consent is granted on the phone during setup. Contact support to re-request a sync — the phone will prompt again.             |
| History import seems stuck             | Large accounts import in phases; give it time. If nothing arrives after a few hours, contact support.                                 |
| Popup never offers the app path        | Confirm the number is on the WhatsApp **Business** app (7+ days) and the app is up to date. Regional availability is decided by Meta. |
