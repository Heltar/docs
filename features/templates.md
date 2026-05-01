---
title: Templates
description: Create message templates
icon: FileText
order: 2
---

# Templates

Message templates are pre-approved messages required for initiating conversations outside the 24-hour window.

**Navigation**: Settings -> **Template Manager** (or Settings menu item #4)

> [!INFO]
> All templates must be approved by Meta before use.

## Template Categories

### Utility

For transactional messages like:

- Order confirmations
- Shipping updates
- Appointment reminders
- Account notifications

### Marketing

For promotional content like:

- Product announcements
- Special offers
- Re-engagement messages
- Newsletters

### Authentication

For security-related messages:

- OTP codes
- Login verification
- Password resets
- Two-factor authentication

## Creating Templates

### Basic Structure

Every template can have:

1. **Header** (Optional)
   - Text (up to 60 characters)
   - Image, Video, or Document

2. **Body** (Required)
   - Main message content
   - Up to 1024 characters
   - Support for variables `{{1}}`, `{{2}}`, etc.
   - Basic formatting (bold, italic)

3. **Footer** (Optional)
   - Additional text (up to 60 characters)
   - Often used for opt-out instructions

4. **Buttons** (Optional)
   - Call to Action (URL or Phone)
   - Quick Reply buttons

### Step-by-Step Creation

1. Navigate to **Templates** page
2. Click **Create Template**
3. Fill in the details:

```
Name: order_shipped
Category: Utility
Language: English (US)

Header: Image (product photo)

Body:
Hi {{1}}! 📦

Great news! Your order #{{2}} has been shipped.

Tracking Number: {{3}}
Expected Delivery: {{4}}

Track your package using the button below.

Footer: Reply STOP to unsubscribe

Buttons:
- Track Order (URL: https://track.example.com/{{5}})
```

4. Preview the template
5. Submit for approval

## Variables

Variables are placeholders replaced with actual values when sending:

| Variable | Example Value |
| -------- | ------------- |
| `{{1}}`  | John          |
| `{{2}}`  | ORD-12345     |
| `{{3}}`  | TRK-98765     |

### Variable Rules

- Variables must be numbered sequentially (1, 2, 3...)
- Each variable needs a sample value for approval
- Variables can't be the entire message content
- Variables in URLs must be at the end

## Formatting

### Supported Formatting

````
*bold text*
_italic text_
~strikethrough~
```monospace```
````

### Example

```
Hello *{{1}}*!

Your _exclusive_ offer expires in ~24 hours~ *12 hours*!

Use code: `SAVE20`
```

## Button Types

### Quick Reply

- Up to 3 buttons
- 20 characters max per button
- User taps to send predefined response

### Call to Action

- Up to 2 buttons
- **URL Button**: Opens a website
- **Phone Button**: Initiates a call

### URL Button Examples

```
Static URL: https://example.com/orders
Dynamic URL: https://example.com/track/{{1}}
```

## Approval Process

### Timeline

- Most templates: 24-48 hours
- Some may take longer during high volume periods

### Common Rejection Reasons

1. **Spelling/Grammar Errors**
   - Proofread carefully before submitting

2. **Misleading Content**
   - Be clear about what you're offering

3. **Missing Required Info**
   - Include business name
   - Provide opt-out instructions for marketing

4. **Policy Violations**
   - No prohibited content (gambling, adult, etc.)
   - No threatening or abusive language

### Resubmitting

If rejected:

1. Review the rejection reason
2. Make necessary corrections
3. Submit again with a new name

## Template Management

### Editing Templates

- Approved templates cannot be edited
- Create a new template with changes
- Delete the old one after new approval

### Template Status

| Status   | Meaning                      |
| -------- | ---------------------------- |
| Pending  | Awaiting Meta review         |
| Approved | Ready to use                 |
| Rejected | Not approved, see reason     |
| Paused   | Temporarily disabled by Meta |
| Disabled | Permanently disabled         |

## Advanced Templates

### Carousel Template

Show multiple cards with images:

```
Carousel:
  Card 1: Image + Body + Buttons
  Card 2: Image + Body + Buttons
  Card 3: Image + Body + Buttons
```

### Flow Template

Interactive multi-step forms:

```
Flow Button -> Opens WhatsApp Flow
  - Multi-screen forms
  - Data collection
  - Appointment booking
```

### Limited Time Offer

```
Limited Time Offer component:
  - Expiration text (max 16 chars)
  - Optional expiration timer
```

## Template Library

Use Meta's pre-approved templates:

1. Go to **Template Manager**
2. Click **Template Library**
3. Browse available templates
4. Select and customize

## Bulk Import

Import multiple templates at once:

**Navigation**: Integrations -> **Bulk Import Template**

## Best Practices

1. **Keep it concise** - Shorter messages have better engagement
2. **Personalize** - Use variables for customer names
3. **Clear CTA** - Make the action obvious
4. **Test thoroughly** - Preview before submitting
5. **Plan ahead** - Submit templates before you need them
6. **Localize** - Create templates in your customers' languages
