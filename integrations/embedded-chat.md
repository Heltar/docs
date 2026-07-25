---
title: Embedded agent chat
description: Drop the agent conversation window into your CRM so agents reply to a lead in place
icon: Inbox
order: 11
---

# Embedded agent chat

Let your agents open a full conversation **for a specific lead** directly inside your own CRM — same thread, composer, templates, attachments and live updates as your inbox. It loads as a draggable, resizable panel that floats over your CRM.

This is the **agent-facing** surface (your team replying to a customer). For a website visitor chat bubble, use the [Web chat widget](./web-widget) instead.

---

## How it works

1. Your CRM backend requests a one-time link for a lead's phone number.
2. You get back a ready-made `embedUrl`. The lead's number and the session token never travel in it — only an opaque, single-use reference does.
3. Your CRM frontend opens that link in the embedded panel. The agent chats; messages send from your WhatsApp number exactly like the dashboard.

---

## 1. Create an API key

In **Settings → API keys**, create a key scoped to **`embed:write`** (or `*`). This key authorises minting embed sessions for your business.

> Keep this key on your **server**. Never ship it to the browser.

## 2. Mint a session (server-to-server)

When an agent opens a lead, call this from your backend with the API key:

```http
POST {{API_URL}}/v1/embed/session
Authorization: Bearer hk_live_xxx
Content-Type: application/json

{
  "clientWaNumber": "919999999999"
}
```

> Only `clientWaNumber` is required. Optionally add `"agentEmail": "agent@yourco.com"` (see below).

Response:

```json
{
  "data": {
    "embedUrl": "{{DASHBOARD_URL}}/embed/inbox?ref=cr_8f2a…",
    "expiresAt": "2026-06-25T20:00:00.000Z"
  }
}
```

- `clientWaNumber` — the lead's WhatsApp number (any of `919999999999`, `+91 99999 99999` — it's normalised). **This is the only required field.**
- `agentEmail` _(optional)_ — the email of the agent opening the chat. When it matches a team member in your business, the session is **attributed to that agent** — replies send under their name and the session shows up under them in your active-sessions list. Omit it (or pass an email that isn't a team member) for a plain business-level session.
- `encrypt` _(optional, default `true`)_ — when `true`, the lead's number never reaches the browser: every API/realtime payload carries an opaque, per-conversation reference instead, so your agents can chat without seeing the actual phone number. Set `"encrypt": false` only if you explicitly want the real number visible in the panel.
- `ui` _(optional)_ — per-session panel options. Everything is opt-in; omit `ui` (or any field) to keep today's behaviour. Because you mint a fresh session each time an agent opens a lead, you control all of this per open:
  - `sessionExpiredText` — free text (1–200 chars), shown **verbatim** in place of the **Send template** button whenever the 24-hour reply window is closed for this session. Any wording, any language. Typical use: your CRM already triggered the template automatically, so instead of prompting agents to send another one you show "Please wait while the customer replies". A customer reply reopens the normal composer automatically.
  - `hideCallButton` — `true` hides the call button in the panel header.
  - `hideChatOptions` — `true` hides the **chat options** menu (assignment, open/close, bot controls, etc.) in the panel header.
  - `showRefresh` — `true` adds a refresh button in the panel header that re-fetches the conversation on demand — handy as a manual fallback on flaky agent networks (messages otherwise arrive in realtime).

  ```json
  {
    "clientWaNumber": "919999999999",
    "ui": {
      "sessionExpiredText": "Please wait while the customer replies",
      "hideCallButton": true,
      "hideChatOptions": true,
      "showRefresh": true
    }
  }
  ```

  > Try all of this without writing code: **Settings → Embedded chat** in your dashboard has a **Try it live** panel — enter a lead's number, set the panel options with toggles, open a live preview (floating, inline, or a new tab), and copy a cURL snippet that carries your exact configuration.

- `embedUrl` is **single-use** and short-lived. Mint a fresh one each time an agent opens a lead.

## 3. Open the panel (frontend)

Include the loader once, then call `open()` with the `embedUrl` you got in step 2:

```html
<script src="{{DASHBOARD_URL}}/embed-chat.js" defer></script>
<script>
  // when your agent clicks "Chat" on a lead:
  EmbedChat.open({ embedUrl: EMBED_URL_FROM_STEP_2, theme: 'dark' });
  // the panel header shows the lead's name automatically
</script>
```

> The hosts above (`{{DASHBOARD_URL}}` for the script + panel, `{{API_URL}}` for the API) are filled in automatically with your account's hosts — both are served for you; you host neither.

**Match your CRM's theme.** Pass `theme: 'dark' | 'light'` so the chat renders in the same mode as your dashboard. Leave it out and the chat follows the agent's own saved preference (or their OS dark-mode setting). The embedded chat can't read your CRM's theme on its own — it runs in a sandboxed frame on a different origin — so pass it explicitly to keep them in sync.

The panel appears in the bottom-right, draggable by its header and resizable from its edges and corners.

### Loader API

| Call                                  | Effect                                                                                                                  |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `EmbedChat.open({ embedUrl })`        | Open / switch the panel to a lead. The header auto-fills with the lead's name. Pass a fresh `embedUrl` to switch leads. |
| `EmbedChat.open({ embedUrl, theme })` | `theme` is `'dark'` or `'light'` — renders the chat in that mode to match your CRM. Optional.                           |
| `EmbedChat.minimize()`                | Collapse to a launcher pill.                                                                                            |
| `EmbedChat.expand()`                  | Restore from the pill.                                                                                                  |
| `EmbedChat.close()`                   | Remove the panel.                                                                                                       |

## Alternative: plain iframe

If you'd rather place the chat in a fixed region (a side drawer) instead of a floating panel, skip the loader and use the `embedUrl` directly:

```html
<iframe
  src="{{DASHBOARD_URL}}/embed/inbox?ref=cr_8f2a…&theme=dark"
  style="width:100%;height:100%;border:0"
  allow="clipboard-read; clipboard-write; microphone; camera; geolocation; autoplay; fullscreen; display-capture; encrypted-media; picture-in-picture; web-share"
></iframe>
```

> Add `&theme=dark` (or `&theme=light`) to the URL to match your CRM's mode — the same control the loader's `theme` option uses. Omit it to follow the agent's own preference.

---

## Notes

- **Short-lived.** The session expires after 24 hours (same as a dashboard login) and each `embedUrl` is consumed on first open — reopening after that needs a fresh one from step 2.
- **Privacy.** The lead's phone number and the session token are never placed in the URL; only an opaque, single-use reference is.
- **Theme.** Pass `theme: 'dark' | 'light'` (loader) or `&theme=dark|light` (iframe URL) to match your CRM's mode; otherwise the chat follows the agent's saved or OS preference.
- **Sending rules.** WhatsApp's 24-hour window applies as usual — outside it, agents send an approved template, just like the dashboard.
