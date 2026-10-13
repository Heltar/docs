---
title: Billing
description: Manage subscription and payments
icon: CreditCard
order: 6
---

# Billing & Wallet

Manage your account balance, payments, and billing information.

**Navigation**: Settings -> Wallet & Billing

## Wallet Overview

Your wallet stores credits for messaging and services.

### Balance Display

| Field                 | Description                  |
| --------------------- | ---------------------------- |
| **Available Balance** | Credits ready to use         |
| **Pending Credits**   | Being processed              |
| **Reserved**          | Held for scheduled campaigns |
| **Total Spent**       | Lifetime usage               |

### Low Balance Alert

Set up notifications:

1. Go to **Wallet** -> **Settings**
2. Set threshold amount
3. Enable email/SMS alerts
4. Get notified when balance drops

## Adding Credits

### Payment Methods

| Method                | Processing Time |
| --------------------- | --------------- |
| **Credit/Debit Card** | Instant         |
| **UPI**               | Instant         |
| **Net Banking**       | 1-24 hours      |
| **Bank Transfer**     | 1-3 days        |

### Adding Funds

1. Go to **Wallet** -> **Add Credits**
2. Enter amount
3. Select payment method
4. Complete payment
5. Credits added to wallet

### Minimum Amounts

| Method        | Minimum  | Maximum      |
| ------------- | -------- | ------------ |
| Card/UPI      | Rs 500   | Rs 10,00,000 |
| Net Banking   | Rs 1,000 | Rs 50,00,000 |
| Bank Transfer | Rs 5,000 | No limit     |

## Message Pricing

### How WhatsApp Messages Are Charged

WhatsApp is charged **per message**, by category. There is no 24-hour conversation fee.

| Category           | What it covers                                                                                                   |
| ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| **Marketing**      | Template messages: promotions, offers, announcements                                                             |
| **Utility**        | Template messages: order updates, confirmations, alerts                                                          |
| **Authentication** | Template messages: OTPs, verification codes                                                                      |
| **Service**        | Free-form replies (no template) you send inside the 24-hour customer service window. Charged from 1 October 2026 |

> [!INFO]
> Rates depend on the category and the recipient's country. Your per-message rates appear on your invoice, and **Settings -> Development & Analytics -> Cost Analytics** shows what each category has cost so far.

### Not Charged

- Messages your customers send you
- Internal notes
- Messages that fail to send

## Transaction History

### Viewing Transactions

1. Go to **Wallet** -> **History**
2. View all transactions:
   - Credits added
   - Message charges
   - Refunds

### Transaction Details

| Field       | Description               |
| ----------- | ------------------------- |
| Date        | When transaction occurred |
| Type        | Credit, Debit, Refund     |
| Amount      | Transaction amount        |
| Balance     | Resulting balance         |
| Reference   | Transaction ID            |
| Description | What it was for           |

### Filtering Transactions

Filter by:

- Date range
- Transaction type
- Amount range
- Campaign (for debits)

### Exporting History

1. Apply filters
2. Click **Export**
3. Choose format (CSV/PDF)
4. Download

## Invoices

### Monthly Invoices

Invoices generated monthly for:

- Platform subscription
- Message charges
- Additional services

### Downloading Invoices

1. Go to **Billing** -> **Invoices**
2. Find invoice by date
3. Click **Download**
4. PDF downloads

### Invoice Details

Each invoice includes:

- Invoice number
- Billing period
- Itemized charges
- Tax details (GST)
- Total amount

## Credit Lines

Pre-approved credit for campaigns.

### Applying for Credit

1. Go to **Settings** -> **Attach Credit Line**
2. Submit application:
   - Business documents
   - KYC details
   - Expected usage
3. Wait for approval

### Credit Terms

| Term         | Description             |
| ------------ | ----------------------- |
| **Limit**    | Maximum credit amount   |
| **Period**   | Days before payment due |
| **Interest** | Rate if payment delayed |

### Using Credit

- Automatically used when wallet empty
- Must be repaid within term
- Late payment may suspend account

## Subscription Plans

### Plan Comparison

| Feature          | Starter | Growth   | Enterprise |
| ---------------- | ------- | -------- | ---------- |
| Monthly Messages | 10,000  | 100,000  | Unlimited  |
| Team Members     | 3       | 10       | Unlimited  |
| Chatbots         | 1       | 5        | Unlimited  |
| API Access       | Basic   | Full     | Full       |
| Support          | Email   | Priority | Dedicated  |

### Changing Plans

1. Go to **Billing** -> **Subscription**
2. Click **Change Plan**
3. Select new plan
4. Confirm change

Upgrades: Immediate
Downgrades: End of billing cycle

### Cancellation

1. Go to **Billing** -> **Subscription**
2. Click **Cancel**
3. Provide reason (optional)
4. Confirm cancellation

Data retained for 30 days after cancellation.

## Tax Information

### GST Details

Update GST for proper invoicing:

1. Go to **Billing** -> **Tax Information**
2. Enter GSTIN
3. Add business address
4. Save

### Tax Calculation

| Component       | Rate    |
| --------------- | ------- |
| Platform Fee    | 18% GST |
| Message Credits | 18% GST |

GST breakdown shown on invoices.

## Refunds

### Refund Policy

| Reason          | Eligible     |
| --------------- | ------------ |
| Failed messages | Yes          |
| Unused credits  | Case by case |
| Subscription    | Pro-rated    |

### Requesting Refund

1. Go to **Billing** -> **Support**
2. Click **Request Refund**
3. Select transactions
4. Provide reason
5. Submit request

Processing time: 5-10 business days

### Refund Methods

- Original payment method
- Wallet credits (faster)

## Payment Security

### Secure Payments

- PCI DSS compliant
- SSL encryption
- 2FA for large transactions

### Saved Cards

Manage saved payment methods:

1. Go to **Billing** -> **Payment Methods**
2. View saved cards
3. Add new or remove existing

### Auto-Recharge

Set up automatic top-up:

1. Enable auto-recharge
2. Set threshold (e.g., Rs 1,000)
3. Set recharge amount (e.g., Rs 5,000)
4. Select payment method
5. Balance auto-maintains

## Best Practices

1. **Monitor balance** - Set low balance alerts
2. **Budget campaigns** - Estimate costs before sending
3. **Use auto-recharge** - Avoid service interruption
4. **Download invoices** - Keep for records
5. **Update GST** - Ensure proper tax credits
6. **Review charges** - Check transaction history monthly
