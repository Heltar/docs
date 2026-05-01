---
title: Contacts
description: Manage your contacts
icon: Users
order: 3
---

# Contacts

Manage all your WhatsApp contacts in one place. View, search, import, and export your customer database.

**Navigation**: Click **Contacts** in the left sidebar

---

## Interface Layout

### Header Section

| Element                         | Description                                    |
| ------------------------------- | ---------------------------------------------- |
| **Users icon + "All Contacts"** | Page title                                     |
| **Search box**                  | Filter contacts by name or phone number        |
| **Export button**               | Download contacts as CSV                       |
| **Create Tags button**          | Open tag management                            |
| **Add Contacts dropdown**       | New Contact or Bulk Upload options             |
| **Direct Message button**       | Message a number without saving (desktop only) |
| **Three-dot menu**              | Create/Update/Delete Attributes                |

### Contact Table

A data table showing all contacts with columns:

- **Sr No.** - Sequential number
- **Name** - Contact name
- **WhatsApp Number** - Phone number (hover to copy)
- **Created At** - When contact was added
- **Tags** - Colored tag badges
- **Custom Attributes** - Your defined fields

### Mobile View

- Stacked card layout instead of table
- "Send message without saving number" card visible
- Icon-only buttons in header

---

## Adding Contacts

### Add Single Contact

1. Click **Add Contacts** button
2. Select **New Contact**
3. Fill in the form:
   - **Name** (required)
   - **WhatsApp Number** with country code (required)
   - Custom attribute fields (based on your setup)
4. Click **Submit**

### Bulk Upload (CSV)

1. Click **Add Contacts** button
2. Select **Bulk Upload**
3. Upload your CSV file

#### CSV Requirements

- **Required columns**: Name, Country_Code, WhatsApp_Number
- Click **Download sample CSV** for template

#### CSV Format Example

```csv
Name,Country_Code,WhatsApp_Number,Email,Company
John Doe,91,9876543210,john@example.com,Acme Inc
Jane Smith,1,5551234567,jane@example.com,Tech Corp
```

> [!INFO]
> Duplicate phone numbers will update existing contacts (upsert behavior).

---

## Direct Message (Without Saving)

Send a message to a number without adding it to your contacts:

### Desktop

1. Click **Direct Message** button (outline style)
2. Enter phone number with country code
3. Click **Start**
4. You'll be redirected to Inbox with that chat open

### Mobile

1. Tap the "Send message without saving number" card
2. Enter phone number with country code
3. Tap **Start**

---

## Searching Contacts

### Search Box

- Type name or phone number in the search box
- Results filter in real-time
- Matching text is highlighted

### Table Filters

- Use column header filters for advanced filtering
- Available on each column

---

## Managing Attributes

Custom attributes let you store additional information about contacts.

### Create Attribute

1. Click **three-dot menu** in header
2. Select **Create Attribute**
3. Choose type:
   - **Text** - Free text field
   - **Numerical** - Number field
   - **Dropdown** - Predefined options
4. Enter attribute name
5. For Dropdown: Add your options with **+** button
6. Click **Submit**

### Update Attribute

1. Click **three-dot menu** in header
2. Select **Update Attribute**
3. Choose the attribute to modify
4. Make changes
5. Click **Submit**

### Delete Attribute

1. Click **three-dot menu** in header
2. Select **Delete Attribute**
3. Choose the attribute to delete
4. Type the attribute name to confirm
5. Click **Submit**

> [!WARNING]
> Deleting an attribute removes it from all contacts. This cannot be undone.

---

## Managing Tags

### Create Tags

1. Click **Create Tags** button in header
2. Enter tag name
3. Choose tag color
4. Click **Create**

### Apply Tags to Contact

1. Click on a contact row to open chat
2. In the contact details panel, click **Edit contact Tags**
3. Select tags to apply
4. Click **Save**

---

## Exporting Contacts

### Export All Contacts

1. Click **Export** button (FileUp icon)
2. A CSV file downloads automatically

### Export Contents

The exported CSV includes:

- Name
- WhatsApp Number
- Created At
- Tags (as third column)
- All custom attributes

> [!INFO]
> Phone numbers may be masked based on your permission settings.

---

## Clicking on a Contact

When you click on a contact row:

1. The app navigates to **Inbox**
2. Opens the chat with that contact
3. Shows conversation history

---

## Permissions

Access to contact features depends on your role:

| Permission             | Feature                  |
| ---------------------- | ------------------------ |
| `viewAllContacts`      | View the contacts table  |
| `exportAllContacts`    | Export contacts to CSV   |
| `contactNumberMasking` | Phone numbers are masked |

---

## Tips

> [!TIP]
> Use bulk upload to import contacts from other systems. Just match the CSV format.

> [!TIP]
> Create meaningful attributes like "Customer Type", "Order Count", or "Subscription Date" to segment your contacts.

> [!TIP]
> Tags are useful for quick filtering in the Inbox. Create tags like "VIP", "Support", "Sales Lead".
