---
title: Export to Amazon S3
description: Get every message status change as CSV files in your own S3 bucket, every 10 minutes
icon: CloudUpload
order: 12
---

# Export to Amazon S3

Every 10 minutes, HeltarChat writes a CSV file to your own Amazon S3 bucket. Each file holds every outgoing message whose status changed in that 10-minute window, one row per message with its full current state. Load the files into Redshift, Athena, Snowflake or your data lake.

Set it up in **Settings → Data Export**. It takes about 10 minutes in the AWS console.

---

## What you get

- **One file per 10-minute window** (UTC), written about 3–8 minutes after the window ends. A window with no changes writes no file.
- **Outgoing messages only:** templates, session messages and bot replies. Incoming customer messages are not exported, but a customer's reply to your message appears in that message's `reply` column.
- **One row per message per file,** with its latest state. A message appears again in a later file when its status changes (sent → delivered → read). Keep the row with the newest `updated_at` for each `message_id`.
- **Nothing is missed.** If writing fails (a changed policy, a deleted bucket), we keep retrying and catch up on every window once it works again. Turning the export off and back on also resumes where it stopped.

### File layout

```
s3://<bucket>/<folder>/messages/business_id=<id>/date=2026-10-10/20261010T0940Z_0950Z.csv
```

- `date=` is the UTC day of the window; the file name is the window's start and end, in UTC.
- The `key=value` folders let Athena, AWS Glue and Redshift Spectrum read the tree as a partitioned table.
- If a window is ever re-exported, its file is overwritten with the same name, never duplicated.

### Columns

Times are ISO 8601 in UTC, like `2026-10-10T09:41:07.123Z`. An empty cell means the event hasn't happened.

| Column              | Meaning                                                                          |
| ------------------- | -------------------------------------------------------------------------------- |
| `message_id`        | WhatsApp message ID (`wamid…`)                                                   |
| `heltar_message_id` | Our message ID                                                                   |
| `phone_number`      | Recipient's phone number, with country code                                      |
| `message_type`      | `template`, `text`, `image`, `interactive`, …                                    |
| `template_name`     | Template name, for template messages                                             |
| `template_category` | `MARKETING`, `UTILITY` or `AUTHENTICATION`                                       |
| `template_language` | Template language code, like `en`                                                |
| `status`            | Current status: `sent`, `delivered`, `read`, `failed`, `responded`, `clicked`, … |
| `created_at`        | When the message was sent                                                        |
| `sent_at`           | When WhatsApp accepted it                                                        |
| `delivered_at`      | When it reached the phone                                                        |
| `read_at`           | When it was read                                                                 |
| `failed_at`         | When it failed                                                                   |
| `clicked_at`        | When a link in it was clicked                                                    |
| `replied_at`        | When the customer replied                                                        |
| `failure_reason`    | Why it failed, from WhatsApp                                                     |
| `reply`             | The customer's reply text                                                        |
| `updated_at`        | When this row last changed. Use it to keep the newest row per `message_id`.      |

The first line of each file is the header. Values with commas, quotes or line breaks are quoted as standard CSV.

---

## Set up

Before you start, create the bucket (or pick an existing one) and note its **region**. In **Settings → Data Export**, click **Set up Amazon S3 export** and enter the bucket name, region and a folder (default `heltar`). Every file goes under that folder, and the permissions below only allow writing there.

Then pick how HeltarChat gets access.

### Option A: Access key

1. In **IAM → Users**, create a user (for example `heltarchat-export`) with no console access.
2. Attach the permission policy shown on the setup page:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": ["s3:PutObject", "s3:AbortMultipartUpload"],
         "Resource": "arn:aws:s3:::<bucket>/<folder>/*"
       }
     ]
   }
   ```

3. Under **Security credentials**, create an access key for "Application running outside AWS".
4. Paste the **access key ID** and **secret access key** into the setup page, then click **Test connection** and **Save**.

The secret is stored encrypted and never shown again. To change other settings later, type the access key ID again; leave the secret blank to keep the saved one.

### Option B: IAM role

No secret is shared: you create a role in your account that our export role may use, locked to an **external ID** generated for you.

1. In the AWS console, open **IAM → Roles → Create role** and choose **Custom trust policy**.
2. Paste the trust policy shown on the setup page. It names our export role and your external ID, already filled in:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "AWS": "<our export role ARN, shown on the setup page>"
         },
         "Action": "sts:AssumeRole",
         "Condition": {
           "StringEquals": { "sts:ExternalId": "<your external ID>" }
         }
       }
     ]
   }
   ```

3. Skip the permissions step, name the role (for example `heltarchat-export`) and create it.
4. Open the role, choose **Add permissions → Create inline policy → JSON**, and paste the permission policy from the setup page (the same one as in Option A).

5. Copy the role's **ARN** into the setup page, then click **Test connection** and **Save**.

The test checks that the role works with your external ID **and refuses it without**. If the role can be used without the external ID, the setup page asks you to add the `sts:ExternalId` condition before it can be saved.

### Buckets encrypted with your own KMS key

If the bucket's default encryption uses a customer-managed KMS key, also allow `kms:GenerateDataKey` on that key for the role or user:

```json
{
  "Effect": "Allow",
  "Action": "kms:GenerateDataKey",
  "Resource": "arn:aws:kms:<region>:<account>:key/<key-id>"
}
```

### Recommended: clean up unfinished uploads

Large files are uploaded in parts. If an upload is interrupted, S3 keeps the unfinished parts (invisible, but billed) until they're cleaned up. Add a lifecycle rule to the bucket: open **Management → Create lifecycle rule**, limit it to the export folder, and under the rule actions choose **Delete expired object delete markers or incomplete multipart uploads**, then **Delete incomplete multipart uploads** after 1 day.

You can also add a rule that expires old export files, if you don't need them after loading.

---

## Load into Redshift

Load every file as it arrives into an append-only table, and read the newest row per message through a view. Re-loading a file, or a window that was re-exported, then never double-counts a message.

```sql
COPY whatsapp_messages_raw
FROM 's3://<bucket>/<folder>/messages/business_id=<id>/date=2026-10-10/'
IAM_ROLE '<your Redshift role ARN>'
CSV IGNOREHEADER 1 TIMEFORMAT 'auto' EMPTYASNULL;

CREATE VIEW whatsapp_messages AS
SELECT *
FROM (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY message_id ORDER BY updated_at DESC) AS row_rank
  FROM whatsapp_messages_raw
)
WHERE row_rank = 1;
```

Create `whatsapp_messages_raw` with the 18 columns above, in the same order. Use `TIMESTAMPTZ` for the `_at` columns (the times end in `Z`) and `VARCHAR` for the rest (`reply` and `failure_reason` can be long, so give them `VARCHAR(65535)`).

---

## Manage the export

- **Status:** the settings page shows how far the export has got, the last file written and when the next run is.
- **Turn off / on:** the switch on the destination. Turning it back on resumes from where it stopped.
- **Edit:** change the bucket, folder or access method at any time. Each change is tested before it's saved.
- **Delete:** stops exporting and removes the settings. Files already in your bucket stay there.
- **Alerts:** if writing keeps failing for an hour, team members who turned on **Settings → Email Alerts → Webhook failure alerts** get an email.

## Troubleshooting

| Message                                                               | Fix                                                                                       |
| --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Bucket not found                                                      | Check the bucket name.                                                                    |
| The bucket is in a different region                                   | Choose the region the bucket was created in.                                              |
| Not allowed to write to this bucket and folder                        | The permission policy is missing, or its `Resource` doesn't match the bucket and folder.  |
| Couldn't use this role                                                | The trust policy must name our export role and require your external ID exactly as shown. |
| This role can be used without your external ID                        | Add the `sts:ExternalId` condition from the trust policy shown.                           |
| The bucket encrypts files with a KMS key…                             | Allow `kms:GenerateDataKey` on that key.                                                  |
| AWS does not recognise this access key ID / The secret does not match | Re-check the key pair, or create a new one.                                               |

> [!NOTE]
> Every test writes a small file named `_heltar_connection_test.txt` into your folder. It's overwritten on each test and safe to delete.
