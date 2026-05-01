---
title: Inbox
description: Manage conversations
icon: Inbox
order: 1
---

# Inbox

The Inbox is your central hub for managing all WhatsApp conversations. It uses a three-panel layout on desktop and a swipeable single-panel layout on mobile.

**Navigation**: Click **Inbox** in the left sidebar

---

## Interface Layout

### Desktop View (3 Panels)

| Panel      | Width           | Description                       |
| ---------- | --------------- | --------------------------------- |
| **Left**   | 25% (min 400px) | Chat list with search and filters |
| **Center** | Flexible        | Active conversation messages      |
| **Right**  | 34%             | Contact details (toggleable)      |

### Mobile View

Swipe between three views:

1. **Chat List** - All conversations
2. **Messages** - Active chat
3. **Contact Details** - Contact info panel

---

## Chat List Panel (Left)

### Header

- **Brand Name** displayed at top
- **New Chat** button (MessageSquarePlus icon) - Opens dialog to message a new number

### Search Box

- Search by customer name or phone number
- Results highlight matching text in bold

### Chat Items

Each chat shows:

- User avatar
- Contact name or phone number
- Last message preview (truncated)
- Timestamp (e.g., "now", "12:30 PM", "Yesterday")
- Unread count badge (green circle)
- Assigned employee color indicator

### Infinite Scroll

- Loads more chats as you scroll down
- Uses virtualized rendering for performance

---

## Filter Options Bar

Located above the chat list with expandable options.

### View Tabs

- **All Chats** - Shows all conversations
- **Your Chats** - Shows only your assigned chats (based on role)

### Status Filters

| Filter       | Icon              | Description            |
| ------------ | ----------------- | ---------------------- |
| Open Chats   | MessageCircleMore | Active conversations   |
| Closed Chats | MessageCircleOff  | Resolved conversations |

### Option Filters

| Filter     | Icon              | Description                |
| ---------- | ----------------- | -------------------------- |
| Unread     | MessageSquareDot  | Chats with unread messages |
| Assigned   | MessageSquarePlus | Chats assigned to someone  |
| Unassigned | MessageSquareX    | Chats not assigned         |
| Bot On     | Bot               | Bot-enabled chats          |
| Bot Off    | BotOff            | Bot-disabled chats         |

### Tag Filters

- Custom tags appear as colored buttons
- Click to filter by specific tags

### Actions

- **Add Tags** button - Opens tag management
- **Clear Filters** button - Resets all filters

> [!TIP]
> Click the **funnel icon** to enable multi-select mode for combining multiple filters.

---

## Messages Panel (Center)

### Header Bar

Shows:

- **Back arrow** (mobile) - Returns to chat list
- **User avatar and name** - Click to toggle contact details panel
- **Open/Closed tabs** - Toggle conversation status
- **Bot On/Off tabs** - Toggle bot for this chat (if enabled)
- **Chat Options dropdown** - Assignment and settings

### Chat Options Dropdown

Contains:

- **Assignment Section** - Assign chat to team members
  - Shows current assignees
  - Multi-select employees
  - "Unassign Chat" option
- **Open/Close Chat** toggle
- **Bot Status** toggle

### Message Display

Messages appear chronologically with:

- **Incoming messages** - Light gray, left-aligned
- **Outgoing messages** - Green, right-aligned
- **Private notes** - Yellow background (team only)
- **Timestamps** on each message
- **Status icons** (clock, single check, double check, blue checks)

### Message Actions (Hover)

Hover over any message to see:

- **Reply** icon - Quote and reply to message
- **Forward** icon - Forward to another contact
- **More options** - Emoji reactions, copy, delete

### Date Separators

Shows dates like "Today", "Yesterday", "Jan 15" between message groups.

---

## Message Input Box

### Input Area

- Multi-line text input
- **Shift+Enter** for new line
- **Enter** to send (desktop)

### Input Features

| Feature      | Icon      | Description              |
| ------------ | --------- | ------------------------ |
| Emoji Picker | Smile     | Insert emojis            |
| Templates    | FileText  | Send approved templates  |
| Attachments  | Paperclip | Send files and media     |
| Private Note | Lock      | Toggle private note mode |
| Send         | Send      | Send the message         |

### Attachment Menu Options

Click the paperclip icon to see:

1. **Document** - PDF, DOC, XLS files
2. **Photos & Videos** - Images and video files
3. **Camera** - Capture photo/video
4. **Template** - Send WhatsApp template
5. **Audio** - Audio files
6. **Contact** - Share contact card
7. **Location** - Share location
8. **Catalogue** - Send product from catalog
9. **Interactive** - Buttons, lists, flows
10. **Quick Reply** - Saved quick responses

### Quick Replies

- Type `/` to trigger quick reply suggestions
- Search and select from saved responses

---

## Contact Details Panel (Right)

### Header

- **Close button** (X) - Hides the panel
- **Dropdown menu** with:
  - Edit contact attributes
  - Edit contact tags

### Content

- Contact phone number and name
- Custom attributes (key-value pairs)
- Contact tags
- Media/links shared in conversation

---

## Sending Messages

### Text Messages

1. Type your message in the input box
2. Press **Enter** or click **Send**

### Media Messages

1. Click the **Paperclip** icon
2. Select media type (Photo, Video, Document, etc.)
3. Choose or capture the file
4. Add optional caption
5. Click **Send**

### Template Messages

Use templates when the 24-hour window has expired:

1. Click the **Template** icon
2. Search and select an approved template
3. Fill in variable values (e.g., customer name, order ID)
4. Click **Send**

> [!INFO]
> The 24-hour messaging window resets each time the customer sends you a message.

### Private Notes

Private notes are visible only to your team:

1. Click the **Lock** icon (turns yellow)
2. Type your internal note
3. Send - it won't be sent to the customer

---

## Assigning Chats

### Assign to Team Member

1. Open the chat
2. Click **Chat Options** dropdown in header
3. Select team member(s) from the list
4. Click checkmark to confirm

### Unassign Chat

1. Open the chat
2. Click **Chat Options** dropdown
3. Click **Unassign Chat** (red text at top)

---

## Keyboard Shortcuts

| Shortcut        | Action               |
| --------------- | -------------------- |
| `Enter`         | Send message         |
| `Shift + Enter` | New line in message  |
| `/`             | Open quick replies   |
| `Esc`           | Close panels/dialogs |

---

## Mobile Features

### Floating Action Button

- Green **+** button at bottom right
- Links to Contacts page for new conversations

### Swipe Navigation

- Swipe left/right to navigate between chat list, messages, and contact details

### Touch Gestures

- Long press on messages for more options
- Pull down to refresh chat list
