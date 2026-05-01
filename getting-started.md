---
title: Getting Started
description: Set up your account and send your first message
icon: Rocket
order: 2
---

# Getting Started

Get up and running in 5 simple steps.

---

## Step 1: Sign Up

Create your account on our platform.

1. Visit the signup page
2. Enter your email and password
3. Verify your email
4. Complete your profile

---

## Step 2: Connect WhatsApp

Go to **Settings** → **WhatsApp API Setup** to connect your WhatsApp Business Account.

> [!NOTE]
> Initially, all details on this page will be blank. They auto-fill after completing the Embedded Signup process.

### Signup Types

There are two signup options:

| Signup Type              | When to Use                                                                    |
| ------------------------ | ------------------------------------------------------------------------------ |
| **Heltar Sign-Up**       | Default. Heltar is your BSP. Credit line is attached directly in the platform. |
| **Route Mobile Sign-Up** | Route Mobile is your BSP. Credit line is attached by Route Mobile on request.  |

> [!NOTE]
> The signup flow is identical for both types. The only differences are: (1) you click **Route Mobile** instead of **Heltar Sign-Up** in Step 2a, and (2) for Route Mobile signups, you request Route Mobile to attach the credit line instead of doing it yourself in Step 2h.

### 2a. Start Embedded Signup

1. Open [Settings > WhatsApp API Setup](/settings/1)
2. Click **Heltar Sign-Up** or **Route Mobile** (depending on your BSP)
3. A Facebook popup will open - log in using your Facebook account, or choose the currently logged-in account

### 2b. Select Business Portfolio

1. Click **Get Started**
2. Select your **Business Portfolio** from the **Meta Business Manager** dropdown
   - If you don't have one, you can create a new portfolio here

### 2c. Enter Business Details

1. Enter your **Business Name**
2. Enter your **Business Website** or **Profile Page**
3. Select your **Country of Operation**
4. Click **Next**

### 2d. WhatsApp Business Account

1. Choose an existing **WhatsApp Business Account** or create a new one
2. Click **Next**

### 2e. Display Name & Category

1. Enter the **Business Name** and the **Display Name** (this is what customers will see)
2. Select the **Category** that best describes your business (if unsure, choose **Other**)
3. Click **Next**

> [!TIP]
> Ensure the Display Name matches the Business Name as closely as possible. Meta may reject names that don't match.

### 2f. Verify Phone Number

1. Enter the phone number you want to register for WhatsApp Business API
2. Select your preferred OTP method: **Text** or **Call**
3. Enter the OTP and click **Next**

> [!WARNING]
> The phone number must NOT already be linked to any existing WhatsApp or WhatsApp Business account. If it is, delete the account from the app first and wait 5 minutes.

> [!NOTE]
> The OTP can only be sent once or twice within a 24-hour period. If you exhaust the attempts, you will need to wait 24 hours before trying again.

### 2g. Complete Signup

1. On the confirmation screen, click **Finish**
2. The Embedded Signup process is now complete

### 2h. Attach Credit Line

**For Heltar Sign-Up:**

1. Go to **Billing and Payments** in the left sidebar
2. Click **Attach Credit Line**
3. Click the green **Attach Credit Line** button to confirm

**For Route Mobile Sign-Up:**

1. Contact your Route Mobile account manager and request them to attach the credit line to your WABA
2. Route Mobile will share their credit line with your account on Meta's end
3. Once attached, your account will be ready to send messages

### Verify Setup

Refresh the page and confirm that all details in the **WhatsApp API Setup** section are automatically filled in.

:::support
Having trouble with the setup? [Contact Customer Support](https://wa.me/917483384786) - we're here to help!
:::

---

## Step 3: Create a Template

Templates are required to start new conversations. Create one in the Template Manager.

1. Go to **Template Manager** from the sidebar
2. Click **Create Template**
3. Fill in the details:
   - **Name**: e.g., `hello_world`
   - **Category**: Choose Utility or Marketing
   - **Language**: Select your language
   - **Body**: Your message (e.g., "Hello! Thanks for connecting with us.")
4. Click **Submit** - Meta will review and approve it

> [!NOTE]
> Template approval usually takes a few minutes to 24 hours. Simple utility templates are approved faster.

---

## Step 4: Send Your First Message

Test your setup by sending a template message to yourself.

1. Go to **Inbox** from the sidebar
2. Click **New Chat** (+ button)
3. Enter your own WhatsApp number (with country code, e.g., 919876543210)
4. Click the **Template** button
5. Select your approved template
6. Click **Send**

---

## Step 5: Check Your Phone

Open WhatsApp on your phone and verify that you received the message.

**Message received?** You're all set!

**Didn't receive it?** Check these:

- Is your template approved? (Check Template Manager for status)
- Did you enter the correct phone number?
- Is your WhatsApp API setup complete?

:::support
Still having issues? [Contact Customer Support](https://wa.me/917483384786) - we'll help you get started!
:::

---

## Optional: Set Up Voice Calling

If you want your AI chatbot to **place phone calls** — over WhatsApp or a regular phone number via SIP — set these up separately. Both are optional; you can enable either, both, or neither.

### WhatsApp Voice Calls

WhatsApp voice calls work out of the box once **Step 2 (WhatsApp API Setup)** above is complete — nothing extra to configure.

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

Now that you're set up, explore these features:

| Feature                                          | Description                                  |
| ------------------------------------------------ | -------------------------------------------- |
| [Inbox](/docs/features/inbox)                    | Manage all your conversations                |
| [Templates](/docs/features/templates)            | Create and manage message templates          |
| [Chatbots](/docs/features/chatbots)              | Automate responses with AI                   |
| [Voice Calls](/docs/features/calls)              | Incoming / outgoing voice calls + voice bot  |
| [SIP Calling Setup](/docs/features/settings/sip) | Connect a SIP trunk for outbound phone calls |
| [API](/docs/api)                                 | Integrate with your systems                  |
