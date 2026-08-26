---
title: Business Username
description: Claim and manage your WhatsApp business username
icon: AtSign
order: 11
---

# Business Username API

A business username is a searchable handle mapped one-to-one to your WhatsApp
business phone number, so people can find you by typing it exactly. Adopting one
does **not** hide your business phone number.

Format rules: 3–35 characters, English letters, digits, period and underscore
only, at least one letter, no leading, trailing or doubled period, must not
start with `www`, must not end with something that looks like a domain.

---

## Authentication

All endpoints require a valid API key in the `Authorization` header.

```bash
Authorization: Bearer YOUR_API_KEY
```

See [Authentication](/docs/api/authentication) for full setup instructions.

---

:::api
method: GET
endpoint: /v1/business/username
title: Get Username
description: Get the username currently attached to your business phone number.

```response
{
  "success": true,
  "message": "Username fetched",
  "data": {
    "username": "jaspersmarket",
    "status": "approved"
  }
}
```

`status` is `approved` when the username is visible to WhatsApp users, or
`reserved` when it is held for you but not yet visible. `username` is omitted
when you have not claimed one.
:::

---

:::api
method: GET
endpoint: /v1/business/username/suggestions
title: Get Reserved Usernames
description: List the usernames WhatsApp has reserved for your business. These have a higher chance of approval.

```response
{
  "success": true,
  "message": "Username suggestions fetched",
  "data": {
    "data": [{ "username_suggestions": ["jaspersmarket", "jaspers.market"] }]
  }
}
```

:::

---

:::api
method: POST
endpoint: /v1/business/username
title: Claim or Change Username
description: Claim a username for your business phone number, or move one you already hold.

## Body Parameters

- username: string [required] - The username you want, following the format rules above
- transferAction: string - `none` (default) or `force_transfer`. Use `force_transfer` when the username is currently on another of your business phone numbers and you want to move it here

```request
{
  "username": "jaspersmarket"
}
```

```response
{
  "success": true,
  "message": "Username requested",
  "data": { "status": "approved" }
}
```

If the username is already in use on another of your business phone numbers,
the request fails until you resend it with `"transferAction": "force_transfer"`.
:::

---

:::api
method: DELETE
endpoint: /v1/business/username
title: Delete Username
description: Release the username attached to your business phone number.

```response
{
  "success": true,
  "message": "Username deleted",
  "data": { "success": true }
}
```

:::
