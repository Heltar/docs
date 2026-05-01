---
title: Team Management
description: Manage team members and roles
icon: Users
order: 2
---

# Team Management

Add team members, assign roles, and configure permissions for your organization.

**Navigation**: Settings -> Manage Team Members

## Adding Team Members

### Invite New Member

1. Go to **Settings** -> **Manage Team Members**
2. Click **Add Member**
3. Enter member details:
   - Name
   - Email address
   - Role
4. Send invitation

The invitee will receive an email with login credentials.

### Member Information

| Field      | Description                      |
| ---------- | -------------------------------- |
| **Name**   | Display name in the platform     |
| **Email**  | Login email (must be unique)     |
| **Role**   | Permission level                 |
| **Status** | Active, Pending, or Disabled     |
| **2FA**    | Two-factor authentication status |

## Roles & Permissions

### Available Roles

| Role        | Description                              |
| ----------- | ---------------------------------------- |
| **Admin**   | Full access to all features and settings |
| **Manager** | Full feature access, limited settings    |
| **Agent**   | Assigned conversations only              |
| **Viewer**  | Read-only access                         |

### Permission Matrix

| Feature            | Admin | Manager | Agent    | Viewer |
| ------------------ | ----- | ------- | -------- | ------ |
| Inbox - All Chats  | Yes   | Yes     | Assigned | View   |
| Inbox - Assign     | Yes   | Yes     | No       | No     |
| Contacts - View    | Yes   | Yes     | Assigned | View   |
| Contacts - Edit    | Yes   | Yes     | No       | No     |
| Contacts - Import  | Yes   | Yes     | No       | No     |
| Templates - Create | Yes   | Yes     | No       | No     |
| Templates - Use    | Yes   | Yes     | Yes      | No     |
| Campaigns - Create | Yes   | Yes     | No       | No     |
| Campaigns - Send   | Yes   | Yes     | No       | No     |
| Chatbots - Edit    | Yes   | Yes     | No       | No     |
| Settings           | Yes   | Limited | No       | No     |
| Analytics          | Yes   | Yes     | Own      | No     |
| API Keys           | Yes   | No      | No       | No     |

### Custom Roles

Create custom roles for specific needs:

1. Go to **Settings** -> **Team Permissions**
2. Click **Create Custom Role**
3. Name your role
4. Toggle permissions for each feature
5. Save

## Two-Factor Authentication

### Enabling 2FA

1. Go to member's profile
2. Click **Enable 2FA**
3. Member scans QR code with authenticator app
4. Enter verification code to confirm

### Supported Authenticator Apps

- Google Authenticator
- Microsoft Authenticator
- Authy
- 1Password

### Enforcing 2FA

Admins can require 2FA for all team members:

1. Go to **Settings** -> **Security**
2. Enable **Require 2FA for all users**
3. Set grace period for existing users

## Managing Members

### Edit Member

1. Find member in the list
2. Click **Edit**
3. Update details or role
4. Save changes

### Disable Member

Temporarily revoke access:

1. Find member in the list
2. Click **Disable**
3. Member cannot log in
4. Re-enable anytime

### Remove Member

Permanently remove access:

1. Find member in the list
2. Click **Remove**
3. Confirm deletion
4. Member's data is preserved for audit

> [!WARNING]
> Removing a member cannot be undone. Their assigned conversations will become unassigned.

## Activity Tracking

### Audit Log

View team member activities:

- Login/logout times
- Messages sent
- Settings changes
- Campaign actions

### Per-Agent Metrics

Track individual performance:

| Metric                | Description            |
| --------------------- | ---------------------- |
| Conversations Handled | Total chats assigned   |
| Messages Sent         | Outbound message count |
| Avg Response Time     | Time to first reply    |
| Resolution Rate       | Chats closed vs total  |
| Customer Satisfaction | If CSAT is enabled     |

## Best Practices

1. **Principle of least privilege** - Give minimum required permissions
2. **Regular audits** - Review team access quarterly
3. **Enable 2FA** - Especially for admins
4. **Use descriptive names** - Clear role naming
5. **Document custom roles** - Maintain permission documentation
6. **Offboard promptly** - Remove access immediately when needed
