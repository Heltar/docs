---
title: Campaigns
description: Send bulk messages
icon: Send
order: 4
---

# Bulk Messaging

Send WhatsApp template messages to multiple contacts at once. Track delivery, engagement, and performance for each campaign.

**Navigation**: Click **Bulk Messaging** in the left sidebar

---

## Interface Overview

The Bulk Messaging page has two main views:

1. **Campaign List** - View all your campaigns and their statistics
2. **Create Campaign** - Build and send a new campaign

---

## Campaign List View

### Campaign Table Columns

| Column            | Description                         |
| ----------------- | ----------------------------------- |
| **Campaign Name** | Name you gave the campaign          |
| **Status**        | draft, schedule, running, sent      |
| **Schedule At**   | Scheduled send time (if applicable) |
| **Template Name** | Which template was used             |
| **Waiting**       | Contacts pending send               |
| **Failed**        | Failed deliveries                   |
| **Sent**          | Successfully sent                   |
| **Delivered**     | Message delivered                   |
| **Read**          | Message read by contact             |
| **Responded**     | Contacts who replied                |
| **Clicked**       | Link clicks (if template has links) |
| **Total**         | Total contacts targeted             |
| **Date Created**  | When campaign was created           |

### Campaign Status Colors

| Status       | Color   | Description         |
| ------------ | ------- | ------------------- |
| **Draft**    | Gray    | Not yet sent        |
| **Schedule** | Blue    | Scheduled for later |
| **Running**  | Amber   | Currently sending   |
| **Sent**     | Emerald | Completed           |

### Campaign Actions

Click on a campaign row to see detailed statistics:

#### Left Panel - Stats Navigation

Shows counts and percentages for each status:

- Waiting, Failed, Sent, Delivered, Read, Responded, Clicked

Click a status to filter contacts by that status.

#### Middle Panel - Contact List

Shows contacts filtered by selected status with:

- Contact name/number
- Profile avatar
- Last message preview

#### Right Panel - Contact Details

Click a contact to see:

- Full interaction history
- Timestamps for each status change
- Failure reasons (if applicable)
- **Go to Chat** button to open conversation

### Refresh Options

Click the refresh dropdown to update statistics:

- **Refresh All Rows** - Update all campaigns
- **Refresh Top 50 Rows** - Quick refresh
- **Refresh Filtered Rows** - Only visible rows

---

## Creating a Campaign

Click the **Create Campaign** button to open the campaign builder.

### Step 1: Campaign Details

Fill in:

- **Campaign Name** (required) - A name to identify this campaign
- **Campaign Description** (optional) - Notes about the campaign

### Step 2: Select Contacts

Toggle between two methods:

#### Option A: Existing Contacts

1. Toggle to **Existing**
2. Click **Select contacts**
3. Use search and filters to find contacts
4. Check the contacts you want
5. Click **Add** to confirm

#### Option B: Upload CSV

1. Toggle to **CSV**
2. Drag and drop a CSV file (or click to browse)
3. View the uploaded data preview

**CSV Requirements:**

- Must include phone numbers
- Can include custom columns for template variables

Click **Download sample CSV** for a template file.

### Step 3: Select Template

1. Search or browse available templates
2. Templates show:
   - Name (with search highlighting)
   - Language
   - Status (green dot = Approved, orange = Pending)
   - Category
3. Click the **eye icon** to preview
4. Click a template to select it

> [!WARNING]
> Only **Approved** templates can be used for campaigns.

### Step 4: Configure Phone Numbers

Set up how phone numbers are formatted:

#### Phone Column

Select which column contains phone numbers.

#### Country Code Options

| Option                       | When to Use                                 |
| ---------------------------- | ------------------------------------------- |
| **Already in mobile column** | Numbers like 917764993992 or +91 7764993992 |
| **Apply same code to all**   | All numbers need the same country code      |
| **In a separate column**     | CSV has a dedicated country code column     |

> [!TIP]
> Click the **sparkles icon** to use AI Auto-map. AI will detect the phone column and country code settings automatically.

### Step 5: Map Template Variables

Match your data columns to template variables:

1. Each template variable (like {{1}}, {{2}}) appears as a dropdown
2. Select which column maps to each variable
3. Use **Show values** toggle to preview actual values

#### For Media Templates

If your template has media (image, video, document):

- Select a column containing media URLs, OR
- Upload a file directly

> [!TIP]
> Click **AI Auto-map** to automatically detect and map all variables. Green sparkle = high confidence, Orange = review suggested.

### Step 6: Review and Send

#### Preview Section

- Use the **row selector** to preview different contacts
- Toggle **Show values** to see actual variable values
- Verify the phone number format is correct

#### Duplicate Handling

If duplicates are found:

- **Remove duplicates** (default) - Shows count of removed
- **Include anyway** - Click to keep duplicates

#### Send Options

| Button                       | Action                 |
| ---------------------------- | ---------------------- |
| **Schedule** (Calendar icon) | Set a future date/time |
| **Send** (Arrow icon)        | Send immediately       |

### Scheduling a Campaign

1. Click **Schedule**
2. Select date and time
3. Must be at least 2 minutes in the future
4. Click **Schedule**

---

## Campaign Confirmation Screen

Before sending, review your campaign:

### Left Panel - Contact List

- Scrollable list of all contacts
- Warning icons for contacts with missing variables
- Click to preview individual messages

### Center Panel - Message Preview

- Full template preview with values filled in
- Shows exactly what contact will receive

### Right Panel - Statistics

- Total contact count
- Contacts with empty variables (warnings)
- Estimated success rate

### Final Actions

- **Send to Backend** - Sends via standard API
- **Send to Webhook** - Sends via configured webhook (if set up)

---

## Retry Failed Contacts

When messages fail to deliver, you can retry them as a new campaign — with all original data preserved.

### How Retry Works

1. Open the campaign
2. Click on **Failed** status in the left panel
3. Review the failed contacts and failure reasons
4. Click the **retry icon** (circular arrow)
5. A new campaign is created with the failed contacts pre-loaded

### Smart Retry Naming

Retry campaigns are automatically named based on the status you're retrying:

| Original Campaign     | Retry From | New Campaign Name     |
| --------------------- | ---------- | --------------------- |
| Diwali Sale           | Failed     | Failed by Diwali Sale |
| Failed by Diwali Sale | Failed     | Failed by Diwali Sale |
| Summer Promo          | Sent       | Sent by Summer Promo  |

The system strips any existing status prefix before adding the new one, so retry chains stay clean.

### Retry CSV Handling

When you retry, the system automatically:

1. **Fetches the original CSV** from your campaign
2. **Filters to only failed contacts** using exact phone number matching
3. **Appends missing contacts** — if a contact exists in the failed list but not in the original CSV, it's added with empty fields
4. **Fills client attributes** — appended contacts are populated with existing data from your contact database (name, city, etc.)
5. **Increments the file name** — retry files are named `retry1_`, `retry2_`, `retry3_`, etc. to track retry attempts

### Retry from Any Status

You can retry contacts from any status, not just failed:

- **Failed** — contacts where delivery failed
- **Sent** — contacts where message was sent but not delivered
- **Waiting** — contacts stuck in the queue

### Download Retry CSV

Instead of retrying directly, you can also download the filtered CSV:

1. Click the **download icon** on the failed contacts panel
2. The CSV is filtered to only include the failed contacts
3. You can edit the CSV manually and re-upload for a new campaign

### Failure Reasons

Each failed contact shows a specific failure reason with a dedicated icon:

- Message delivery errors
- Invalid phone numbers
- Rate limiting issues
- Template-specific errors

> [!TIP]
> Check failure reasons before retrying. If contacts failed due to invalid numbers, retrying won't help — remove them from the list first.

---

## Message Status Tracking

### Status Progression

Each message progresses through these statuses:

```
Waiting → Sent → Delivered → Read → Responded
                                  → Clicked → Clicked & Responded
```

| Status                  | Description                            |
| ----------------------- | -------------------------------------- |
| **Waiting**             | Queued, not yet sent                   |
| **Failed**              | Delivery failed                        |
| **Sent**                | Sent to WhatsApp servers               |
| **Delivered**           | Delivered to contact's phone           |
| **Read**                | Contact opened the message             |
| **Responded**           | Contact replied                        |
| **Clicked**             | Contact clicked a link in the template |
| **Clicked & Responded** | Contact clicked and also replied       |

> [!INFO]
> A contact at "Read" status has also passed through "Sent" and "Delivered". The left panel shows cumulative counts — clicking "Delivered" shows all contacts that were delivered (including those already read or responded).

---

## Export Campaign Statistics

Export detailed statistics for analysis:

1. Open a completed campaign
2. Click **Export** button
3. Choose which status to export (or all)
4. CSV downloads with:
   - Contact name
   - WhatsApp number
   - Status
   - Responded message (if any)
   - Failure reason (if applicable)

---

## Delete a Campaign

Only **scheduled** campaigns can be deleted:

1. Select the campaign
2. Click **Delete** button (red)
3. Confirm deletion

> [!WARNING]
> Sent campaigns cannot be deleted as they are part of your message history.

---

## Best Practices

> [!TIP]
> Test your campaign with a small group first before sending to everyone.

> [!TIP]
> Use meaningful campaign names like "Jan 2024 - Product Launch" for easy tracking.

> [!TIP]
> Check the **Waiting** count after sending. If contacts are stuck, there may be rate limiting.

> [!TIP]
> Monitor **Responded** count to measure engagement and follow up with interested customers.
