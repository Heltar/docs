---
title: Meta AI
description: Meta's AI agent answering customers on your WhatsApp number
icon: Sparkles
order: 10
---

# Meta AI

Meta AI (the Meta Business Agent) is an AI agent **built and run by Meta** that answers your customers directly on WhatsApp. Unlike a regular chatbot, the conversation is handled on Meta's side — you configure what the agent knows and how it behaves, publish it to your number, and monitor everything from the dashboard: its replies, delivery receipts, handoffs to your team, and quality evaluations.

**Navigation**: Click **Meta AI** in the left sidebar (desktop only)

---

## Eligibility

Meta gates who can run a Business Agent. All of the following must be true for a number:

### 1. Business vertical

Your WhatsApp business profile's **vertical** must be one of the five Meta allows:

- Automotive
- Consumer Packaged Goods
- Professional Services
- Retail & E-commerce
- Travel

Verticals such as Finance, Government, Health, Education, Non-profit or "Other" are **not eligible**. You can change the vertical in WhatsApp Manager (or from your WhatsApp profile settings) — after changing it, eligibility can take a little while to refresh.

### 2. Terms of Service — accepted once per business portfolio

The **Meta Business AI Terms of Service** must be accepted for your business. This is done in **WhatsApp Manager**, where a "Meta Business Agent" section appears once at least one of the portfolio's numbers is eligible. Accepting it **once covers every WhatsApp Business Account under that business portfolio** — numbers owned by a different portfolio need their own acceptance.

Until the ToS is accepted, every check on the number fails with a Terms-of-Service error even if the vertical is correct.

### 3. Number-level eligibility

Even with the vertical and ToS in place, Meta approves each phone number individually. The Meta AI page checks this automatically — an ineligible number shows a **Not eligible** badge with Meta's reason.

### 4. Language and regional rollout

The agent's language is determined by the **business phone number**, and Meta supports a limited set of languages and countries while the feature rolls out. Capabilities can also differ per number — the **Diagnostics** panel (in the agent editor) shows exactly what works on yours, and its report can be copied for a Meta support ticket.

---

## The Meta AI page

The page mirrors the AI Agent dashboard, filtered to Meta AI agents:

- A grid of your Meta AI agents with their status.
- **Create Meta AI Agent** — start a new agent from scratch.
- **Import from Meta** — adopt an agent that was already configured directly in WhatsApp Manager.
- Status badges: **Live on Meta** (the agent is answering customers), **Not eligible** (hover for Meta's reason), **Sync error** (the last configuration push failed).

---

## Creating an agent

1. Click **Create Meta AI Agent**, give it a name, and open it.
2. Configure the sections — everything here is what the agent knows and how it behaves:

| Section           | What it does                                                                                                                                                                   |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Settings**      | Handoff to a human (on/off + the message customers see), follow-up messages, who the agent answers (everyone or an allowlist of numbers), and phrases the agent must never say |
| **Business info** | Facts the agent quotes: business description, return policy, payment methods, delivery & shipping, purchase info, contact info                                                 |
| **Skills**        | Task-specific instructions the agent follows                                                                                                                                   |
| **FAQs**          | Question-and-answer pairs the agent can use verbatim                                                                                                                           |
| **Websites**      | Pages the agent crawls for knowledge                                                                                                                                           |
| **Files**         | Documents the agent reads — each is fetched from your link and uploaded to Meta (up to 25 MB per file)                                                                         |
| **Functions**     | Tools the agent can call — the same org functions your chatbots and voice bots use                                                                                             |

3. **Save** the configuration.
4. Follow-up messages support intervals from **5 minutes up to 24 hours** after the customer goes quiet.

The **Live preview** pane shows how the agent's profile and messages will look on WhatsApp while you edit.

---

## Importing an existing agent

If the business already configured Meta AI **directly in WhatsApp Manager**, click **Import from Meta** on the Meta AI page:

- The agent's live configuration — settings, business info, skills, FAQs, websites and its knowledge-file list — is pulled into a local agent you can edit and version like any other.
- If a local Meta AI agent already exists for the number, you'll be asked to **confirm before it is overwritten** with Meta's live config.
- Importing never changes what is live: the agent keeps running exactly as it was until you publish changes.
- Knowledge files that were uploaded directly at Meta keep working after the import; to manage one from the editor, re-add it with a downloadable link.

---

## Going live

- **Publish** the agent (card menu on the Meta AI page) to push its configuration to Meta and switch it on for your number.
- After that, edit → **Save**, then **Deploy** to push changes to the live agent. The editor shows **Agent is live** / **Not published** so you always know the state.
- **Unpublish** stops the agent everywhere.

> **Pausing caveat:** switching the agent off halts it in _all_ conversations. When you switch it back on, Meta only answers **new** conversations — threads that were mid-conversation stay with your team.

---

## How it shows up in the inbox

- Meta AI conversations appear in the inbox like any other chat. The agent's replies are labelled **Meta AI**, along with their delivered/read receipts.
- **Handoff to a human is decided by Meta.** When the agent judges that a person is needed (or the customer asks), it hands the conversation to your team — with the handoff message you configured. You cannot force a takeover from your side while the agent holds the conversation; to stop the agent everywhere, unpublish it.
- After a handoff, your team replies normally. Use **Hand back to Meta AI** in the chat to return the conversation to the agent.
- Your own Heltar chatbot will never answer a conversation the Meta agent is handling — no double replies.

---

## Testing and quality

- **Test message** — send the agent a message and read its answer without touching a real customer conversation.
- **Evaluations** — Meta simulates a customer against your live agent and scores each conversation with a judge model. Evaluation cases are authored in WhatsApp Manager; from the editor you can run them, watch progress, and review the summary, highlights and per-turn breakdown of every conversation.
- **Diagnostics** — per-number capability checks (settings, skills, FAQs, websites, files, business info, evaluations, tools) with a **Working / Unavailable / Not used** verdict for each, a **Recheck** button, and a copyable report for Meta support.
