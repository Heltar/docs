---
title: WhatsApp Setup
description: Configure WhatsApp Business API
icon: MessageCircle
order: 1
---

# WhatsApp Configuration

Set up and manage your WhatsApp Business Account settings.

**Navigation**: Settings -> WhatsApp Profile

## Business Profile

Your WhatsApp Business profile is visible to all customers.

### Profile Fields

| Field             | Max Length | Description             |
| ----------------- | ---------- | ----------------------- |
| **Business Name** | 256 chars  | Your company name       |
| **Description**   | 512 chars  | What your business does |
| **Address**       | 256 chars  | Physical location       |
| **Website**       | 256 chars  | Company website URL     |
| **Email**         | 128 chars  | Contact email           |
| **Category**      | -          | Business category       |

### Business Hours

Set your operating hours:

1. Go to **WhatsApp Profile**
2. Click **Business Hours**
3. Set hours for each day
4. Enable **Away Message** for off-hours

### Profile Photo

Requirements:

- Square image (recommended 640x640)
- JPG or PNG format
- Max 5MB
- Professional, business-appropriate

## Registering WhatsApp API

### Supported Providers

| Provider            | Type      | Description             |
| ------------------- | --------- | ----------------------- |
| **Heltar Direct**   | Cloud API | Direct Meta integration |
| **Embedded Signup** | Cloud API | Quick Facebook signup   |
| **Karix**           | BSP       | Third-party provider    |
| **ValueFirst**      | BSP       | Third-party provider    |
| **Kaleyra**         | BSP       | Third-party provider    |
| **Route Mobile**    | BSP       | Third-party provider    |
| **TataComm**        | BSP       | Third-party provider    |

### Heltar Direct Setup

1. Go to **Settings** -> **Register Meta API**
2. Select **Heltar (Direct)**
3. Enter your credentials:
   - Phone Number ID
   - WhatsApp Business Account ID
   - Access Token
4. Verify connection
5. Complete setup

### Embedded Signup

The fastest way to get started. This flow creates your WhatsApp Business Account, registers your phone number, and connects everything to Heltar automatically.

#### Signup Types

| Signup Type              | When to Use                                                                    |
| ------------------------ | ------------------------------------------------------------------------------ |
| **Heltar Sign-Up**       | Default. Heltar is your BSP. Credit line is attached directly in the platform. |
| **Route Mobile Sign-Up** | Route Mobile is your BSP. Credit line is attached by Route Mobile on request.  |

> [!NOTE]
> The flow is identical for both types. The only differences are: (1) you click **Route Mobile** instead of **Heltar Sign-Up** in Step 1, and (2) for Route Mobile signups, you request Route Mobile to attach the credit line instead of doing it yourself.

#### Prerequisites

- A Facebook account with admin access to a Meta Business Manager (or you can create one during signup)
- A phone number **not** currently registered on WhatsApp or WhatsApp Business app
- The phone number must be able to receive SMS or voice calls for OTP verification

#### Step-by-Step Process

**1. Initiate Signup**

1. Go to **Settings** -> **WhatsApp API Setup**
2. Click **Heltar Sign-Up** or **Route Mobile** (depending on your BSP)
3. Log in with Facebook in the popup that opens

**2. Select Business Portfolio**

1. Click **Get Started**
2. Select your **Business Portfolio** from the Meta Business Manager dropdown
   - You can also create a new portfolio if you don't have one

**3. Enter Business Details**

1. Enter your **Business Name**
2. Enter your **Business Website** or **Profile Page**
3. Select your **Country of Operation**
4. Click **Next**

**4. WhatsApp Business Account**

1. Choose an existing **WhatsApp Business Account** or create a new one
2. Click **Next**

**5. Display Name & Category**

1. Enter the **Business Name** and **Display Name** (shown to customers)
2. Select the **Category** that best describes your business (choose **Other** if unsure)
3. Click **Next**

> [!TIP]
> Ensure the Display Name matches the Business Name as closely as possible. Meta may reject mismatched names.

**6. Verify Phone Number**

1. Enter the phone number to register
2. Choose OTP method: **Text** or **Call**
3. Enter the OTP and click **Next**

> [!WARNING]
> The number must NOT be linked to any existing WhatsApp account. If it is, delete the account from the app first and wait 5 minutes before proceeding.

> [!NOTE]
> The OTP can only be sent once or twice within a 24-hour period. If you exhaust the attempts, you will need to wait 24 hours before trying again.

**7. Complete Signup**

1. Click **Finish** on the confirmation screen

**8. Attach Credit Line**

**For Heltar Sign-Up:**

1. Go to **Billing and Payments** in the left sidebar
2. Click **Attach Credit Line**
3. Click the green **Attach Credit Line** button to confirm

**For Route Mobile Sign-Up:**

1. Contact your Route Mobile account manager and request them to attach the credit line to your WABA
2. Route Mobile will share their credit line with your account on Meta's end
3. Once attached, your account will be ready to send messages

#### Verify Setup

Refresh the page and confirm that all details in the **WhatsApp API Setup** section are now filled in automatically.

## Managing Phone Numbers

### Adding Numbers

1. Go to **Settings** -> **Manage Numbers**
2. Click **Add Number**
3. Enter phone number
4. Complete OTP verification
5. Number is added to your account

### Number Settings

For each number, configure:

| Setting          | Description                   |
| ---------------- | ----------------------------- |
| **Display Name** | Name shown to customers       |
| **Default Bot**  | Chatbot for new conversations |
| **Auto-Assign**  | Agent assignment rules        |
| **Greeting**     | Welcome message               |

### Removing Numbers

1. Find number in the list
2. Click **Remove**
3. Confirm removal

> [!WARNING]
> Removing a number will disconnect it from the platform. Message history is preserved.

## Product Catalog

Enable WhatsApp Commerce features.

### Setting Up Catalog

1. Go to **Settings** -> **Catalog**
2. Connect your Facebook Catalog
3. Or create products manually:
   - Product name
   - Description
   - Price
   - Image
   - SKU

### Catalog Messages

Send product catalog in messages:

1. Open conversation
2. Click **Catalog** icon
3. Select products
4. Send to customer

### Single Product Message

```json
{
  "type": "product",
  "catalog_id": "123456789",
  "product_retailer_id": "SKU-001"
}
```

### Multi-Product Message

```json
{
  "type": "product_list",
  "catalog_id": "123456789",
  "sections": [
    {
      "title": "Featured",
      "product_items": [
        { "product_retailer_id": "SKU-001" },
        { "product_retailer_id": "SKU-002" }
      ]
    }
  ]
}
```

## Account Status

### Verification Status

| Status           | Description              |
| ---------------- | ------------------------ |
| **Verified**     | Green checkmark badge    |
| **Pending**      | Verification in progress |
| **Not Verified** | Standard account         |

### Quality Rating

| Rating     | Description                 |
| ---------- | --------------------------- |
| **Green**  | High quality, good standing |
| **Yellow** | Warning, needs improvement  |
| **Red**    | Low quality, restricted     |

### Maintaining Quality

- Keep spam reports low
- Avoid blocked messages
- Use approved templates
- Respect opt-outs
- Send relevant content

## Webhook Subscription

Subscribe to WhatsApp events:

### Available Events

| Event                         | Description            |
| ----------------------------- | ---------------------- |
| `messages`                    | Incoming messages      |
| `message_status`              | Delivery/read status   |
| `message_reaction`            | Emoji reactions        |
| `phone_number_quality_update` | Quality changes        |
| `account_update`              | Account status changes |

### Subscribing

1. Go to **Settings** -> **Subscribe Apps**
2. Select events to subscribe
3. Confirm subscription

## Best Practices

1. **Complete your profile** - Fill all fields for credibility
2. **Use business photo** - Professional logo or image
3. **Accurate hours** - Set correct business hours
4. **Monitor quality** - Check rating regularly
5. **Keep credentials secure** - Never share API tokens
