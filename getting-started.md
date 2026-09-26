---
title: Getting Started
description: Learn the platform on the sandbox, build and publish your first bot, then add your own number
icon: Rocket
order: 2
---

# Getting Started

Every new account starts with a **sandbox**: a test account on our shared WhatsApp number. You can try everything on it (inbox, contacts, bots, campaigns) without Meta approval, a card, or your own number. When you are ready, you add your own number as a second business, and the sandbox stays for testing.

Read this page top to bottom once. Each step says what to do, what you should see, and why it works that way.

| Step | What you do                       | Time   |
| ---- | --------------------------------- | ------ |
| 1    | Sign up                           | 1 min  |
| 2    | Connect your phone to the sandbox | 1 min  |
| 3    | Chat from the inbox               | 2 min  |
| 4    | Add contacts                      | 2 min  |
| 5    | Build a bot and publish it        | 10 min |
| 6    | Send a campaign                   | 3 min  |
| 7    | Add your own WhatsApp number      | 15 min |

---

## Step 1: Sign Up

1. Enter your email, password and business name.
2. Enter the code we email you.
3. You are signed in straight away and land on the **Welcome** screen.

Your business name becomes your organisation's name. The account you start in is called **Sandbox Business**.

---

## Step 2: Connect Your Phone to the Sandbox

The Welcome screen shows a QR code and a short code like `join k7m3xq`.

1. Scan the QR code with your phone's camera. WhatsApp opens with the message already typed.
2. Tap **Send**.
3. The Welcome screen changes to **Your phone is connected**.

You can find the same QR code any time in **[Settings > WhatsApp API Setup](/settings/1)**.

**Why:** the sandbox number is shared by many accounts. The code tells it which account your phone belongs to. From now on, your phone talks to your account only.

> [!NOTE]
> Up to **5 phones** can connect to one sandbox. To add a sixth, remove one first: in **Settings > WhatsApp API Setup**, click the **✕** next to it. Removing a phone blocks it; its chat and messages stay, and unblocking it brings it back.

> [!TIP]
> When your first phone connects, your organisation receives **$10 of AI credit**. Bots answer from this balance.

---

## Step 3: Chat From the Inbox

1. Open **[Inbox](/inbox)**. Your phone's chat is at the top.
2. Type a message and send it. It arrives on your phone.
3. Reply from your phone. The reply appears in the inbox a second later.

**Why:** this is the loop every feature builds on. Campaigns start conversations, bots answer them, and your team replies here.

> [!NOTE]
> WhatsApp lets a business send free-form messages only within **24 hours** of the customer's last message. After that, you need a template (Step 6). Sending one message from your phone reopens the window.

---

## Step 4: Add Contacts

1. Open **[Contacts](/contacts)** and click **Add Contacts**.
2. Choose **New Contact** for one number, or **Bulk Upload** for a CSV file.
3. Enter the number with its country code, for example `919876543210`.

A contact on the sandbox can receive messages only after that phone has connected (Step 2), and the sandbox counts at most 5 phones. Adding contacts here is how you prepare a list for a campaign.

---

## Step 5: Build a Bot and Publish It

A bot answers new chats for you. Building one has three parts, and each has its own place. Knowing why saves a lot of confusion later.

| Part    | Where         | What happens                                                                                                         |
| ------- | ------------- | -------------------------------------------------------------------------------------------------------------------- |
| Build   | **AI Studio** | You describe the bot; the copilot writes it. Nothing is live yet.                                                    |
| Deploy  | **AI Studio** | The version you choose is saved to your account and appears on the **AI Agent** page. Still not answering customers. |
| Publish | **AI Agent**  | You choose which bot answers your chats. Only one bot answers at a time.                                             |

### 5a. Build it in AI Studio

1. Open **[AI Studio](/ai-studio)**.
2. Tell the copilot what the bot should do, in plain words. For example: _"A friendly assistant for my bakery. Answer questions about opening hours and cakes, and ask for the customer's name before taking an order."_
3. The copilot writes the bot and tests it. Ask it to change anything you do not like.
4. When you are happy, click **Create Version** and write one line about what changed.

**Why versions:** a version is a snapshot. If a later change goes wrong, you can go back to a version that worked.

### 5b. Deploy the version

1. Click **Deploy** and pick the version you just created.
2. Open **[AI Agent](/chatbot)**. Your bot is there, marked **Inactive**.

**Why deploy is separate:** you can keep editing in AI Studio without touching what customers see. Customers only ever meet a version you deployed.

### 5c. Publish it on the AI Agent page

1. On the bot's card, open the **⋮** menu.
2. Click **Publish as Text Bot**.
3. Send a message from your connected phone. The bot answers.

**Why publish is separate:** you may have several bots (a test one, a sales one, a support one). Publishing chooses the one that answers. To stop it, open the same menu and click **Unpublish Text Bot**.

> [!TIP]
> Changing a published bot later is safe: edit it in AI Studio, create a new version, and deploy it. The published bot updates in place and keeps answering.

> [!WARNING]
> If the bot stops answering, check your AI credit first. Bots need a balance to reply.

---

## Step 6: Send a Campaign

1. Open **[Bulk Messaging](/templates)** and click **+ New Campaign**.
2. Pick a template, pick your contacts, and send.
3. Watch each message move through Sent, Delivered and Read.

**Why templates:** WhatsApp only lets a business start a conversation with a message Meta approved in advance. The sandbox comes with a ready-made set, so you can send right away. Creating your own templates becomes available once you add your own number.

> [!NOTE]
> A sandbox campaign can reach at most the 5 connected phones.

---

## Step 7: Add Your Own WhatsApp Number

When you are ready to talk to real customers, add your own number. It becomes a **second business** in your organisation; the sandbox stays untouched for testing, and you switch between them from the business switcher in the sidebar.

1. Click the business switcher above **Settings** in the sidebar, then **Add New Number**.
2. Give it a name and click **Add**. You are switched to the new business on the **WhatsApp API Setup** page.
3. Follow the steps below.

> [!WARNING]
> Always add a new number as a new business. Setting up a different number again inside a business that is already connected replaces its connection.

### Signup Types

**Route Mobile Sign-Up** is the default and the one to use: Route Mobile is the BSP for every new number, and its credit line is attached from within the platform (Step 7h). The other tabs are for businesses onboarding through a different BSP; use one only if that BSP has told you to.

### 7a. Start Embedded Signup

1. Open [Settings > WhatsApp API Setup](/settings/1)
2. Click **Route Mobile Sign-Up**
3. A Facebook popup will open - log in using your Facebook account, or choose the currently logged-in account

### 7b. Select Business Portfolio

1. Click **Get Started**
2. Select your **Business Portfolio** from the **Meta Business Manager** dropdown
   - If you don't have one, you can create a new portfolio here

### 7c. Enter Business Details

1. Enter your **Business Name**
2. Enter your **Business Website** or **Profile Page**
3. Select your **Country of Operation**
4. Click **Next**

### 7d. WhatsApp Business Account

1. Choose an existing **WhatsApp Business Account** or create a new one
2. Click **Next**

### 7e. Display Name & Category

1. Enter the **Business Name** and the **Display Name** (this is what customers will see)
2. Select the **Category** that best describes your business (if unsure, choose **Other**)
3. Click **Next**

> [!TIP]
> Ensure the Display Name matches the Business Name as closely as possible. Meta may reject names that don't match.

### 7f. Verify Phone Number

1. Enter the phone number you want to register for WhatsApp Business API
2. Select your preferred OTP method: **Text** or **Call**
3. Enter the OTP and click **Next**

> [!WARNING]
> The phone number must NOT already be linked to any existing WhatsApp or WhatsApp Business account. If it is, delete the account from the app first and wait 5 minutes.

> [!NOTE]
> The OTP can only be sent once or twice within a 24-hour period. If you exhaust the attempts, you will need to wait 24 hours before trying again.

### 7g. Complete Signup

1. On the confirmation screen, click **Finish**
2. The Embedded Signup process is now complete

### 7h. Credit Line

The credit line for your sign-up type is attached **automatically** right after signup. You will see "Credit line attached" and your number can send messages straight away.

If you see a message that it could not be attached:

1. Go to **Settings > Billing & Payments > Attach Credit Line**
2. Click the green **Attach Credit Line** button

If a credit line is already attached, the page says so and nothing changes. Credit Card SignUp does not use a credit line: you pay Meta with your own card.

### Verify Setup

Refresh the page and confirm that all details in the **WhatsApp API Setup** section are automatically filled in.

:::support
Having trouble with the setup? [Contact Customer Support](https://wa.me/917483384786) - we're here to help!
:::

---

## After Your Number Is Live

- **Templates:** create your own in **[Settings > Template Manager](/settings/4)**. Approval usually takes minutes, sometimes up to 24 hours.
- **Bots:** bots built in the sandbox can be exported from their **⋮** menu and imported in the new business.
- **Team:** invite teammates from **Settings** so they can reply from the shared inbox.

---

## Optional: Set Up Voice Calling

If you want your AI chatbot to **place phone calls** — over WhatsApp or a regular phone number via SIP — set these up separately. Both are optional; you can enable either, both, or neither.

### WhatsApp Voice Calls

WhatsApp voice calls work out of the box once **Step 7 (your own number)** above is complete — nothing extra to configure.

The AI agent can place outbound WhatsApp calls to any WhatsApp number via `POST /v1/calls/initiate` with `callType: "whatsapp", mode: "agent"` — see the **[Calls API](/docs/api/calls)**.

### SIP Calls (regular phone numbers)

To dial **non-WhatsApp** phone numbers (landlines, mobile without WhatsApp), attach a SIP trunk:

1. Get a SIP trunk from Twilio / Telnyx / Vonage or any custom provider.
2. Open **Settings → SIP Calling**.
3. Pick the provider, enter the trunk hostname, credentials, caller-ID number, and transport.
4. Click **Save** — the trunk is provisioned and your business card on the home screen will show a **✓ SIP** badge.

Full walkthrough (fields, transport cheat-sheet, troubleshooting): **[SIP Calling Setup](/docs/features/settings/sip)**.

### Test it

Open any chatbot → **Voice Bot** tab → **Test Call** — pick WhatsApp, Phone (SIP), or Browser to try the agent live. Modes you haven't set up show a warning and a one-click link to the relevant setup page.

---

## What's Next?

| Feature                                          | Description                                  |
| ------------------------------------------------ | -------------------------------------------- |
| [Inbox](/docs/features/inbox)                    | Manage all your conversations                |
| [Templates](/docs/features/templates)            | Create and manage message templates          |
| [AI Agent](/docs/features/chatbots)              | Bots in depth: tools, testing, voice         |
| [Voice Calls](/docs/features/calls)              | Incoming / outgoing voice calls + voice bot  |
| [SIP Calling Setup](/docs/features/settings/sip) | Connect a SIP trunk for outbound phone calls |
| [API](/docs/api)                                 | Integrate with your systems                  |
