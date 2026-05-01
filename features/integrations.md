---
title: Integrations
description: Connect third-party apps
icon: Puzzle
order: 8
---

# Integrations

Connect third-party apps and services to extend platform capabilities.

**Navigation**: Click **Integrations** in the left sidebar

---

## Interface Layout

### Header

- **Integrations** title
- **Add** button - Create custom apps

### Tabs

| Tab           | Description             |
| ------------- | ----------------------- |
| **Installed** | Apps you have installed |
| **All Apps**  | All available apps      |
| **Your Apps** | Apps you created        |

### App Cards

Each app card shows:

- App icon and name
- Star rating
- Install count
- Description
- **Install/Uninstall** button
- **Open** button (after installation)

---

## Built-in Integrations

### CleverTap

Connect CleverTap for user engagement and analytics.

**Navigation**: Integrations -> Install **CleverTap** -> **Open**

**Setup Steps:**

1. Login to CleverTap and click "Explore Demo"
2. Select E-commerce option
3. Go to Settings -> Channels -> WhatsApp
4. Select "WhatsApp Connect" tab
5. Click "Provider Configuration"
6. Choose "Generic" provider
7. Enter the callback URLs:
   - **Delivery Report Callback URL** - For message status
   - **Inbound Message Callback URL** - For incoming messages
8. Copy the HTTP Endpoint from the integration page
9. Add Headers with key and value as shown

**Webhook Fields Used:**

- `cleverTapStatus` - Delivery status events
- `cleverTapMessages` - Message events

#### Import Templates

CleverTap can import WhatsApp templates directly from Heltar using the template import endpoint.

**Setup in CleverTap:**

1. Go to Settings -> Channels -> WhatsApp -> WhatsApp Connect
2. Click "Provider Configuration" -> Select "Generic"
3. Scroll to the **Import Templates** section
4. Configure the following:
   - **Request Type**: GET
   - **HTTP Endpoint**: `{{API_URL}}/v1/integrations/clever-tap/templates`
   - **Authentication**: Add `Authorization` header with your Bearer JWT token

**How it works:**

- CleverTap sends a GET request to the endpoint with optional `limit` and `after` query params
- Heltar fetches templates from Meta's Graph API and returns them in Meta's format
- Pagination is handled automatically - CleverTap follows the `next` URL for subsequent pages
- All pagination URLs point to Heltar's endpoint (not directly to Meta)

**Example Request:**

```
GET /v1/integrations/clever-tap/templates?limit=50
Authorization: Bearer <your_jwt_token>
```

**Example Response:**

```json
{
  "data": [
    {
      "name": "welcome_message",
      "language": "en",
      "status": "APPROVED",
      "category": "MARKETING",
      "components": [...]
    }
  ],
  "paging": {
    "cursors": {
      "before": "...",
      "after": "..."
    },
    "next": "{{API_URL}}/v1/integrations/clever-tap/templates?limit=50&after=..."
  }
}
```

---

### WebEngage

Connect WebEngage for customer engagement and journey builder.

**Navigation**: Integrations -> Install **WebEngage** -> **Open**

**Webhook Field Used:**

- `webEngageStatus` - Status events forwarding

---

### MoEngage

Connect MoEngage for mobile-first customer engagement.

**Navigation**: Integrations -> Install **MoEngage** -> **Open**

**Webhook Field Used:**

- `moEngage` - Incoming messages and delivery status

---

### Shopify

Connect your Shopify store for WhatsApp notifications.

**Navigation**: Integrations -> Install **Shopify** -> **Open**

**Setup Steps:**

1. Login to your Shopify admin
2. Go to Settings -> Notifications
3. Click on Webhooks
4. Click "Create Webhook"
5. Select event type (e.g., checkout creation, order confirmation)
6. Paste the webhook URL (replace BusinessId with your ID)
7. Choose latest Webhook API version
8. Click Save

**Supported Events:**

- Checkout creation
- Order confirmation
- Order fulfillment
- And other Shopify webhook events

---

### Link Tracker

Create short URLs and track clicks.

**Navigation**: Integrations -> Install **Link Tracker** -> **Open**

**Interface:**

- **URL input** - Paste your long URL
- **Send button** - Generate short link
- **Short URL display** - Copy the generated link
- **Links table** - View all created links

**Table Columns:**
| Column | Description |
|--------|-------------|
| Sr No. | Serial number |
| Original URL | The full destination URL |
| Short URL | The shortened tracking URL |
| Created At | When the link was created |

**Click Analytics:**
Click any row to view detailed analytics in an embedded dashboard.

---

### WhatsApp Flows

Build interactive multi-step forms for WhatsApp.

**Navigation**: Integrations -> Install **WhatsApp Flows** -> **Open**

**Features:**

- Create multi-screen forms
- Add form elements:
  - Text/Heading
  - Paragraph
  - Short Answer
  - Multiple Choice (MCQ)
  - Date picker
  - Image
  - Opt-in checkbox
- Preview JSON output
- Edit and manage forms

**Use Cases:**

- Lead capture forms
- Appointment booking
- Surveys and feedback
- Order forms

---

### OpenAI Playground

Test OpenAI voice conversations.

**Navigation**: Integrations -> Install **OpenAI Playground** -> **Open**

**Features:**

- Voice conversation testing
- Parameter configuration sidebar
- Real-time AI responses

---

### Custom OAuth / Auto Login

Create custom apps with OAuth authentication.

**Navigation**: Integrations -> **Add** -> Fill form

**Options:**

- **Auto Login** - SSO token integration
- **Full Page Iframe** - Embed external apps

---

## Creating Custom Apps

### Add New App

1. Click **Add** button in header
2. Fill in app details:
   - App name
   - Description
   - Icon URL
   - Iframe URL
   - Webhook URL (optional)
3. Submit to create

### Your Apps Tab

View and manage apps you created:

- See install count
- Open app settings
- Uninstall from your account

---

## App Store

The **All Apps** tab shows available apps including:

**Coming Soon Apps** (marked with disabled button):

- GPT3 Customer Service
- Shopify Cart Abandonment
- Analytics BI
- Ultra Translate
- Razorpay
- And more...

> [!INFO]
> Apps marked "Coming Soon" are planned features and not yet available.

---

## Webhook Integration

For custom webhook integration, use **Settings** -> **Webhook Manager**.

**Available Webhook Fields:**

| Field                     | Description                      |
| ------------------------- | -------------------------------- |
| `metaWebhooks`            | All Meta webhook events          |
| `cleverTapStatus`         | CleverTap status events          |
| `cleverTapMessages`       | CleverTap message events         |
| `webEngageStatus`         | WebEngage status events          |
| `moEngage`                | MoEngage events                  |
| `metaCustomFieldHook`     | Meta webhooks with custom fields |
| `botWebhook`              | Bot-related webhooks             |
| `frontendCampaignForward` | Campaign payloads                |

See [Developer Tools](/docs/features/settings/developer) for webhook setup details.

---

## Tips

> [!TIP]
> Install an app first, then click **Open** to access its settings.

> [!TIP]
> Use Link Tracker to measure campaign performance with click analytics.

> [!TIP]
> WhatsApp Flows are great for collecting structured data from customers.
