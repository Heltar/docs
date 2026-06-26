---
title: Integrations
description: Step-by-step setup guides for third-party integrations
icon: Puzzle
order: 6
---

# Integrations

Connect third-party tools to the platform and start sending WhatsApp messages from your existing stack. Each guide walks through the exact clicks, fields, and webhook URLs you'll need.

> [!TIP]
> The platform exposes a webhook endpoint per integration. Once you copy the URL into the third-party tool, status events flow back into your inbox automatically.

:::cards
[WebEngage](/docs/integrations/web-engage) - Trigger WhatsApp messages from WebEngage journeys and receive delivery status back.
[CleverTap](/docs/integrations/clever-tap) - Trigger WhatsApp messages from CleverTap campaigns and receive delivery status back.
[Web chat widget](/docs/integrations/web-widget) - Embed an in-page chat that flows into the same inbox as WhatsApp.
[Embedded agent chat](/docs/integrations/embedded-chat) - Drop the agent conversation window into your CRM to reply to a lead in place.
:::

---

## What's available today

| Integration                                             | Use case                                                |
| ------------------------------------------------------- | ------------------------------------------------------- |
| [WebEngage](/docs/integrations/web-engage)              | Customer engagement, journey builder, campaigns         |
| [CleverTap](/docs/integrations/clever-tap)              | Customer engagement, campaigns, journeys                |
| [Web chat widget](/docs/integrations/web-widget)        | In-page chat that flows into the same inbox as WhatsApp |
| [Embedded agent chat](/docs/integrations/embedded-chat) | Agent conversation window embedded in your own CRM      |

More integrations (MoEngage, Shopify, Link Tracker) are configurable from the **App Store** in the dashboard. Dedicated step-by-step guides for those will land here next.

---

## Webhook fields

Every integration writes to a dedicated field on your webhook config so you can route events selectively. See [Webhooks](/docs/api/webhooks) for the full list.
