---
title: SIP Calling Setup
description: Connect a SIP trunk so the voice bot can place calls to regular phone numbers
icon: Phone
order: 8
---

# SIP Calling Setup

Connect a SIP trunk so your AI voice bot can dial regular phone numbers (not just WhatsApp). Without this, the **Phone** mode in the chatbot's Test Call dialog and `POST /v1/calls/initiate` with `callType: "sip"` will not work.

**Navigation**: **Settings** → **SIP Calling**.

---

## Before you start

You need a SIP trunk from one of these providers:

| Provider   | What to grab from their dashboard                                                 |
| ---------- | --------------------------------------------------------------------------------- |
| **Twilio** | Elastic SIP Trunks → Termination URI (e.g. `your-trunk.pstn.twilio.com`) + creds. |
| **Telnyx** | SIP Trunking → FQDN (e.g. `sip.telnyx.com`) + credentials.                        |
| **Vonage** | Voice → SIP Trunking → Termination SIP URI + creds.                               |
| **Custom** | Any SIP provider — you provide the hostname, port, credentials, and transport.    |

You also need:

- The phone number that will be the outbound caller-ID (must be attached to the trunk on the provider side).
- The country code for that number.

> [!TIP]
> Start with your provider's **termination** (outbound) trunk. Inbound trunks are a separate step and not required for outbound voice-bot calls.

---

## Fields on the setup page

| Field               | What goes here                                                                                              |
| ------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Provider**        | `Twilio` / `Telnyx` / `Vonage` / `Custom`. Picks sensible defaults for port and transport.                  |
| **Address**         | Trunk hostname only — no `sip:` prefix, no port. e.g. `your-trunk.pstn.twilio.com`.                         |
| **Username**        | SIP credential username (provider-side).                                                                    |
| **Password**        | SIP credential password. Stored encrypted server-side; left blank in the UI after first save.               |
| **Dial prefix**     | Optional. Digits prepended before the destination national number on dial (e.g. carrier access code `+91`). |
| **Transport**       | `Auto (UDP/TCP)` (default), `UDP`, `TCP`, or `TLS`. Pick `TLS` only if your provider uses port `5061`.      |
| **Business number** | The caller-ID phone number. Uses the same country-code + number input as the rest of the product.           |

Hit **Save** — the outbound trunk is provisioned and linked to your business. That's what `POST /v1/calls/initiate` uses when `callType: "sip"`.

---

## Transport cheat-sheet

| Option         | Encryption | Typical port | When to pick it                                                             |
| -------------- | ---------- | ------------ | --------------------------------------------------------------------------- |
| **Auto**       | None       | 5060         | Default. UDP is tried first, falls back to TCP. Does **not** try TLS.       |
| **UDP**        | None       | 5060         | Fastest, lowest latency. Providers that require UDP.                        |
| **TCP**        | None       | 5060         | Networks that drop UDP, or large SIP messages.                              |
| **TLS (SIPS)** | Yes        | 5061         | Required for port 5061 and any provider that enforces encrypted signalling. |

> [!IMPORTANT]
> Encrypt media? Transport only covers SIP signalling. Media (RTP) encryption (SRTP) is handled by your provider separately.

---

## Testing the setup

Once saved, the sidebar badge on **Manage Business Numbers** shows **✓ SIP**, and the chatbot editor's **Test Call → Phone** tab becomes enabled.

1. Open any chatbot → **Voice Bot** tab → **Test Call**.
2. Choose **Phone**, enter your own number (country code + digits).
3. Click **Call me on phone**.
4. Your phone rings within seconds; pick up to talk to the bot.

If it fails, the error message tells you what to fix — see Troubleshooting.

---

## Troubleshooting

### `SIP trunk not found. Re-run SIP setup...`

The trunk ID stored on your business no longer resolves on the voice infrastructure (trunk removed, wrong project, or partial setup).

**Fix**: open **Settings → SIP Calling**, click **Re-setup** (or save again with the current values). This re-provisions a fresh trunk and stores the new id.

### `SIP createSipParticipant failed: authentication failed`

Credentials are wrong, or the provider-side trunk is IP-restricted and our outbound IPs aren't allow-listed.

**Fix**:

- Double-check username/password in the provider dashboard.
- If the provider does IP allow-listing, ping support — we'll share the current outbound IP ranges so you can add them.

### Call rings for a second then drops

Usually a **caller-ID / allowed-number** mismatch on the provider side — the trunk rejects the `From` number.

**Fix**: confirm the _Business number_ you entered is actually attached to this trunk in the provider dashboard. Twilio, for example, requires the number to be listed under the trunk's "Originating" section.

### `Port 5061` / TLS errors

Transport mismatch.

**Fix**: set **Transport** to `TLS` when the provider uses SIPS on 5061. Set to `Auto` / `UDP` / `TCP` only when the provider is plaintext on 5060.

---

## Tearing down

To remove SIP entirely, click **Remove SIP Trunk** on the same settings page. This:

- Deletes the outbound trunk from the voice infrastructure.
- Clears `sipConfig` on your business record.
- Disables the Phone option in the chatbot Test Call dialog.

Saved SIP credentials are wiped server-side when you tear down; you'll need to re-enter them if you set SIP up again.

---

## Related

- **[Voice Calls → Voice Bot Integration](/docs/features/calls#voice-bot-integration)** — how SIP fits into the outbound voice bot flow.
- **[Calls API](/docs/api/calls)** — `callType: "sip"` request/response shapes.
- **[Chatbots → Test Voice Bot](/docs/features/chatbots#test-voice-bot)** — use the in-editor tester once SIP is connected.
