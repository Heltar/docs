---
title: Voice Calls
description: Make and receive calls
icon: Phone
order: 6
---

# Calls

Manage WhatsApp voice calls directly from the platform. View call history, receive incoming calls, and make outbound calls.

**Navigation**: Click **Calls** in the left sidebar

---

## Interface Layout

### Header Section

- **"Calls"** title
- **Search box** - Filter calls by name or phone number
- **New Call** button - Initiate a call to any number

### Call List

Displays all calls in a responsive grid layout:

- 3 columns on large screens
- 2 columns on medium screens
- 1 column on mobile

Uses virtualized scrolling for performance with many calls.

---

## Call History Display

### Empty State

When no calls exist:

- Phone icon with "No Call History" message
- "Make Your First Call" button to get started

### Call Card Information

Each call card shows:

- **User Avatar** - Contact profile picture or placeholder
- **Contact Name** - Name or phone number
- **Call Type Badge** - Incoming, Outgoing, or Missed
- **Direction Icon** with color coding:
  - Red cross = Missed calls
  - Green arrow down = Incoming calls
  - Blue arrow up = Outgoing calls
- **Timestamp** - When the call occurred
- **Duration** - Call length in MM:SS format
- **Status Badge** - Shows "RINGING" if call is active

### Call Card Actions

- **Phone icon** - Click to call this contact again
- **Three-dot menu** - Access call recording

---

## Making Calls

### New Call Dialog

1. Click **New Call** button in header
2. Enter phone number with country code
3. Click **Call**

> [!INFO]
> Phone number must be at least 5 digits.

### Call from History

Click the phone icon on any call card to call that contact.

---

## Incoming Calls

### Floating Incoming Calls Panel

When calls come in, a floating panel appears showing:

- **Header**: "Incoming Calls (N)" with call count
- **Pulsing green dot** on caller avatars
- **Phone number** (may be masked based on settings)
- **"Voice Bot handling"** indicator (if voice bot is active)
- **Accept button** (green phone icon)
- **Reject button** (red phone-off icon)

### Panel Features

- **Draggable** - Move the panel anywhere on screen
- **Minimizable** - Click minus to collapse to a small pill
- **Edge-snapping** - Minimized pill snaps to screen edges

### Minimized Pill

When minimized, shows:

- Green phone icon with pulsing animation
- Red badge with call count
- Tap to expand back to full panel

---

## Active Call Panel

When you accept a call, a floating panel appears.

### Panel Information

- **Drag handle** - White bar at top for moving
- **Minimize button** (minus icon)
- **Call Type** - "Incoming Call" or "Outgoing Call"
- **Contact Name/Number**
- **Duration Counter** - Shows "Connecting..." then MM:SS

### Call Controls

| Button          | Color | Action             |
| --------------- | ----- | ------------------ |
| **Mute/Unmute** | Green | Toggle microphone  |
| **End Call**    | Red   | Terminate the call |

### Minimized Active Call Pill

- Shows phone icon with pulse animation
- Last 4 digits of phone number
- Current duration or "Connecting..."
- Tap to expand

---

## Call Recordings

### Access Recording

1. Click the **three-dot menu** on a call card
2. Select to open recording dialog
3. **Audio player** appears if recording exists
4. Click **Download Recording** to save

### No Recording Available

If no recording exists, message displays: "No call recording available for this call"

---

## Chat Preview

Click on a call card to open the Chat Preview panel:

- Shows contact information
- Subtitle with call type and duration
- Close button to dismiss
- Full conversation history available

---

## Call Types

| Type         | Icon       | Color | Description                |
| ------------ | ---------- | ----- | -------------------------- |
| **Incoming** | Arrow down | Green | Call received and answered |
| **Outgoing** | Arrow up   | Blue  | Call you made              |
| **Missed**   | X mark     | Red   | Call not answered          |

---

## Call Settings

Configure call settings in **Settings** -> **Call Settings**:

### General Settings

- **Enable/Disable calls** - Turn calling on or off
- **Call icon visibility** - Show/hide in chat
- **Timezone settings** - For call hour restrictions

### Business Hours

- Set available hours for receiving calls
- Configure different hours for different days
- Holiday schedule configuration

### Ringtone Settings

- **Enable/Disable ringtone** - Toggle sound
- **Volume control** - Adjust ringtone volume (0-100%)
- **Incoming ringtone** toggle
- **Outgoing ringtone** toggle

---

## Voice Bot Integration

When a Voice Bot is published for your business, calls can be handled entirely by an AI agent — both **incoming** and **outbound**.

### Incoming (bot-handled)

- Incoming calls show the **"Voice Bot handling"** indicator in the panel.
- Bot answers and runs the full conversation, including tool calls (e.g., send a WhatsApp message, end call).
- Bot can hand off to a human agent when it decides it can't help.

### Outbound (AI agent dials the user)

The bot can place calls too — over WhatsApp **or** over a phone number via SIP.

**How to trigger it:**

| Channel  | From the platform                                                                           | From the API                                                                                      |
| -------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| WhatsApp | Chatbot editor → **Voice Bot** tab → **Test Call** → _WhatsApp_.                            | `POST /v1/calls/initiate` with `{callType:"whatsapp", mode:"agent", clientWaNumber, chatbotId}`.  |
| Phone    | Chatbot editor → **Voice Bot** tab → **Test Call** → _Phone_. Requires SIP trunk connected. | `POST /v1/calls/initiate` with `{callType:"sip", mode:"agent", clientWaNumber, chatbotId}`.       |
| Browser  | Chatbot editor → **Voice Bot** tab → **Test Call** → _Browser_. Talk from this tab.         | `POST /v1/calls/test-voice-bot` with `{chatbotId}` — returns a join token for the browser client. |

**Setup requirements:**

- **WhatsApp outbound** — complete **Settings → WhatsApp API Setup** (Meta onboarding).
- **SIP outbound** — attach an outbound trunk in **Settings → SIP Calling**.
- **Browser test** — just allow microphone in the browser.

**Publishing as Voice Bot**

1. Go to **AI Agent**.
2. Create or select a chatbot.
3. Click **Publish** → **Publish as Voice Bot**. This sets the chatbot as the business's default voice bot used when `chatbotId` is omitted in API calls.

> [!TIP]
> See the **[Calls API reference](/docs/api/calls)** for full request/response shapes, error cases, and the `callType × mode` matrix.

---

## Mobile Experience

### Layout Differences

- Single column call list
- Full-width call cards
- Bottom navigation padding (65px)
- Touch-optimized buttons

### Floating Panels

- Same draggable behavior as desktop
- Edge-snapping works on all screen edges
- Animated transitions

---

## Best Practices

> [!TIP]
> Use a headset for better audio quality during calls.

> [!TIP]
> Configure business hours to avoid calls outside working hours.

> [!TIP]
> Enable call recordings for training and quality assurance.

> [!TIP]
> Set up Voice Bot to handle calls when agents are unavailable.

---

## Troubleshooting

### No Incoming Calls?

- Check browser notification permissions
- Verify microphone access is granted
- Ensure call settings are enabled
- Check network connectivity

### Call Not Connecting?

- Verify phone number format (include country code)
- Check if the number is on WhatsApp
- Ensure account is in good standing

### Poor Audio Quality?

- Use stable internet connection
- Close bandwidth-heavy applications
- Try wired connection instead of WiFi
- Use a dedicated headset
