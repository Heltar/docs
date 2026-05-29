---
title: Web chat widget
description: Embed the HeltarChat chat widget on your website
icon: MessageSquare
order: 10
---

# Web chat widget

Paste one `<script>` on your site and visitors get an in-page chat that lands in your HeltarChat inbox — same AI agents and chatbots as WhatsApp. History persists per device, grouped by day like WhatsApp Web.

---

## 1. Allowlist your domain

In **Settings → Web Chat Widget**, add every domain you'll embed on:

```
https://yoursite.com
https://*.yoursite.com
```

Wildcard (`*.`) covers subdomains. HTTPS only.

## 2. Paste the snippet

Your settings page shows this snippet **pre-filled** with your exact values — copy it from there. It looks like:

```html
<script src="https://<YOUR_DASHBOARD_HOST>/web-widget.js" defer></script>
<script>
  window.addEventListener('DOMContentLoaded', function () {
    HeltarChat.initBubble({
      businessId: <YOUR_BUSINESS_ID>,
      apiHost: 'https://<YOUR_API_HOST>',
      theme: {
        primaryColor: '#008069',
        headerTitle: 'Acme Support',
        headerSubtitle: 'We typically reply in 5 minutes',
        welcomeMessage: 'Hi! Ask us anything.',
      },
    });
  });
</script>
```

Two HeltarChat-provided hosts — **Heltar serves both; you don't host either**:

- `<YOUR_DASHBOARD_HOST>` — the HeltarChat dashboard that serves the script bundle (e.g. `app.heltar.com`). This is where `/web-widget.js` lives.
- `<YOUR_API_HOST>` — the HeltarChat API the widget talks to (e.g. `api.heltar.com`). Usually a **different** host from the dashboard, so don't reuse the dashboard host here.

The floating bubble appears in the bottom-right corner.

## 3. Test it

Open the page, click the bubble, send a message. It should appear in your HeltarChat inbox. Agent / AI replies show up in the panel in real time.

---

## Configuration

| Field                   | Type                | Default     | Notes                                                                                               |
| ----------------------- | ------------------- | ----------- | --------------------------------------------------------------------------------------------------- |
| `businessId`            | `number`            | —           | **Required.** From your settings page                                                               |
| `apiHost`               | `string`            | same origin | Base URL of the HeltarChat API                                                                      |
| `mode`                  | `string`            | `light`     | Colour scheme — `light`, `dark`, or `system` (follows the visitor's OS). Switchable live, see below |
| `autoShowDelay`         | `number` ms         | —           | Auto-open the panel N ms after page-load                                                            |
| `visitor.id`            | `string`            | —           | Identify the visitor from your own auth (phone, user id) — see "Identified visitors" below          |
| `visitor.name`          | `string`            | —           | Display name shown to agents in the inbox                                                           |
| `theme.primaryColor`    | `string`            | `#008069`   | Brand colour (bubble, header, send + reply buttons)                                                 |
| `theme.headerTitle`     | `string`            | `Chat`      | Chat header title                                                                                   |
| `theme.headerSubtitle`  | `string`            | —           | One-line subtitle                                                                                   |
| `theme.avatarUrl`       | `string`            | —           | Square ~40px header icon                                                                            |
| `theme.welcomeMessage`  | `string`            | —           | Shown when there's no prior chat                                                                    |
| `theme.placement`       | `string`            | `right`     | Bubble position — `left` or `right`                                                                 |
| `theme.width`           | `number` / `string` | `380px`     | Chat panel width — a number is px, or any CSS length (`32rem`, `90vw`)                              |
| `theme.height`          | `number` / `string` | `620px`     | Chat panel height — a number is px, or any CSS length                                               |
| `theme.launcherSize`    | `number` / `string` | `56px`      | Size of the floating bubble button                                                                  |
| `theme.launcherIconUrl` | `string`            | —           | Custom image on the launcher button (replaces the default chat icon)                                |

## Customize the look

Everything below is optional — pass only what you want to change. Colours,
size and the launcher icon are all set through `theme`:

```html
<script src="https://<YOUR_DASHBOARD_HOST>/web-widget.js" defer></script>
<script>
  window.addEventListener('DOMContentLoaded', function () {
    HeltarChat.initBubble({
      businessId: 123,
      apiHost: 'https://<YOUR_API_HOST>',
      mode: 'system', // 'light' | 'dark' | 'system'
      theme: {
        primaryColor: '#7c3aed', // brand accent (bubble, header, buttons)
        headerTitle: 'Acme Support',
        headerSubtitle: 'We typically reply in 5 minutes',
        welcomeMessage: 'Hi! Ask us anything.',
        avatarUrl: 'https://acme.com/logo-40.png', // header avatar
        placement: 'right', // 'left' | 'right'

        // ── size ──
        width: 420, // panel width — a number is px…
        height: '80vh', // …or any CSS length
        launcherSize: 64, // floating bubble button

        // ── launcher icon ──
        launcherIconUrl: 'https://acme.com/chat-icon.png',
      },
    });
  });
</script>
```

Sizes accept a bare number (treated as pixels) or any CSS length string
(`'32rem'`, `'90vw'`). On small screens the panel is automatically capped to
the viewport, so a large `width`/`height` won't overflow on mobile.

## Light / dark mode

The widget has built-in light, dark and system palettes. Pick one with `mode`, and it follows your site **live** — no reload:

```js
// follow the visitor's OS setting
HeltarChat.initBubble({ businessId: 123, mode: 'system' });

// or bind it to your own light/dark toggle
HeltarChat.setMode('dark'); // 'light' | 'dark' | 'system'
```

If you use the declarative element, flip the attribute instead and the widget re-paints:

```js
document.querySelector('heltar-chat-bubble').setAttribute('mode', 'dark');
```

`theme.primaryColor` re-brands the accent colour in both palettes; `HeltarChat.setTheme({ primaryColor: '#7c3aed' })` changes it live.

## Identified visitors (recommended for logged-in users)

If your site already knows the visitor (logged-in user, a phone you've verified, an internal user id, …), pass that identity to the widget. Agents will find the **same conversation across the visitor's devices and channels** — no surprise duplicate threads.

```js
HeltarChat.initBubble({
  businessId: 123,
  apiHost: 'https://<YOUR_API_HOST>',
  visitor: {
    id: '919876543210', // a phone number, user id, or any stable identifier
    name: 'John Doe',
  },
});
```

What happens under the hood:

- The visitor's chats are saved against `<your-id>@web` (e.g. `919876543210@web`) instead of an anonymous random id.
- The same `visitor.id` from a different device → same conversation thread.
- Agents searching the inbox by phone / user id will find the chat instantly.

**Format constraints on `visitor.id`:**

- 4 – 128 characters
- Only `[A-Za-z0-9_-]` (letters, digits, underscore, hyphen)
- Phone numbers and UUIDs satisfy this; emails (because of `@` and `.`) do **not** — hash them first or use your own user id

**When to set it:**

Set `visitor.id` **before** calling `initBubble`. If you change it later in the page session, the widget won't switch threads automatically — call `HeltarChat.unmount()` then `HeltarChat.initBubble({...})` again with the new id.

## Runtime control (`window.HeltarChat`)

After the script loads, drive the widget from your own UI:

| Method               | What it does                                                           |
| -------------------- | ---------------------------------------------------------------------- |
| `initBubble(props)`  | Mount the widget (see Configuration). Call once.                       |
| `open()` / `close()` | Expand / collapse the chat panel.                                      |
| `unmount()`          | Remove the widget entirely.                                            |
| `setMode(mode)`      | Switch between `light`, `dark` and `system` **live** — no re-init.     |
| `setTheme(theme)`    | Merge theme overrides (colour, size, icon, …) into the running widget. |

```js
HeltarChat.open(); // expand the panel
HeltarChat.close(); // collapse
HeltarChat.unmount(); // remove the widget

HeltarChat.setMode('dark'); // flip the colour scheme live
HeltarChat.setTheme({ primaryColor: '#7c3aed' }); // re-brand live
```

```html
<button onclick="HeltarChat.open()">Chat with us</button>
```

## Embed methods

| Method                       | Snippet                                                                                                                          |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Script tag** (most common) | `<script src=".../web-widget.js"></script>` + `HeltarChat.initBubble({...})`                                                     |
| **Declarative element**      | `<heltar-chat-bubble business-id="123" api-host="..."></heltar-chat-bubble>`                                                     |
| **Self-host the bundle**     | Build `dist/web.js` from the [web-widget repo](https://github.com/Heltar/web-widget) and serve it from your own host — see below |

### Self-host the bundle

By default the widget loads from your Heltar dashboard at `/web-widget.js`. To serve it yourself instead, build it from the public repo and host the output:

```bash
git clone https://github.com/Heltar/web-widget.git
cd web-widget && npm install && npm run build   # → dist/web.js
```

Host `dist/web.js` on your own CDN and point your `<script src>` at it. Self-hosting only changes where the _script_ is served from — `apiHost` must still point at the Heltar API, and the page's origin must still be allowlisted under **Settings → Web Chat Widget**.

---

## Visitor identity & history

The widget assigns a unique ID to each visitor and stores it in their browser's first-party storage. When the same device returns, the conversation history loads automatically — Safari ITP, Brave, and Chrome's third-party-cookie phaseout don't affect it.

Clearing the visitor's browser data starts a new thread.

## Security

- **Origin allowlist** — every request must come from a domain you've added in settings. All others are rejected.
- **Visitor identity is browser-local** — each visitor gets a random id stored only in their own browser's first-party storage; it never leaves their device except in requests from THEIR widget.
- **Disable instantly** — clear all domains in **Settings → Web Chat Widget** to disable the widget for every visitor immediately. No new visitor can connect; existing ones get cut off on their next request.

---

## Troubleshooting

| Symptom                             | Fix                                                                                                                               |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Bubble never appears                | Snippet running before `defer`-loaded script finished — wrap in `DOMContentLoaded`                                                |
| Requests rejected (403)             | The page's origin isn't in your allowlist (Step 1)                                                                                |
| Chat works, history doesn't persist | Visitor's browser is blocking storage (Safari Private mode, embedded webview). Expected — chat works for the current session only |
| AI agent doesn't reply              | Same setup as WhatsApp — enable your AI agent / chatbot for this business in their respective settings                            |

---

## Supported today

Text messages, images & files with captions, quick-reply buttons and list menus, identified visitors (cross-device history), realtime delivery, and read receipts all work — the web channel flows through the same inbox, chatbots and AI agents as WhatsApp.
