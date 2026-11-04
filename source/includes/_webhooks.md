# Public Webhooks & Callbacks

The endpoints in this section are **public**: they do not use the `Authorization: BASIC API_USERNAME:API_PASSWORD` header. Each one is protected by a secret token in its URL (and, where the platform supports it, by the platform's request signature or an OAuth `state` value).

They are called by external services (the Fluent Support email piping server, your own forms/apps, Telegram, Slack, Twilio, Dropbox, Google, Help Scout), not by API users. You normally only copy their URL from the Fluent Support settings screen into the external service. Keep the tokens secret: anyone with the URL can call the endpoint.

## Incoming Webhook (Create Ticket)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/public/incoming_webhook/<webhook_token>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Can not activate my license",
    "content": "<p>I get an error when I activate the license on my site.</p>",
    "priority": "medium",
    "sender": {
        "first_name": "John",
        "last_name": "Doe",
        "email": "john@example.com"
    },
    "custom_fields": {
        "cf_website_url": "https://example.com"
    },
    "attachments": [
        {
            "url": "https://example.com/files/screenshot.png",
            "filename": "screenshot.png",
            "contentType": "image/png"
        }
    ]
}'
```

> The above command creates a new ticket for the sender

```json
{
    "type": "new_ticket",
    "ticket_id": 215,
    "ticket": {
        "id": 215,
        "customer_id": 31,
        "mailbox_id": 1,
        "title": "Can not activate my license",
        "content": "<p>I get an error when I activate the license on my site.</p>",
        "source": "webhook",
        "priority": "medium",
        "client_priority": "medium",
        "status": "new",
        "created_at": "2026-11-04 10:45:12",
        "updated_at": "2026-11-04 10:45:12"
    }
}
```

This endpoint creates a ticket from any external app (a form plugin, Zapier, your own code, ...). The customer is found by `sender.email` or created. The ticket is created in the mailbox selected for the incoming webhook in the settings (the default mailbox if none).

If the sender is an existing customer and the request matches one of their tickets (a `#<ticket number>` reference in the title, or the same title as a ticket updated in the last 30 days in that mailbox), the content is added as a reply to that ticket instead, and the response is `{"type": "new_response", "ticket_id": ..., "response_id": ..., "response": {...}, "customer": {...}}`. A request whose content is identical to an open ticket of the same customer in the same mailbox is ignored (the response body is `false`).

Get the webhook URL (with its token) from **Fluent Support > Settings > Incoming Webhook**; you can also choose the target mailbox and regenerate the token there.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/public/incoming_webhook/<webhook_token>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
sender[email] | email | yes | Customer email. The customer is created if it does not exist
sender[first_name] | string | no | Customer first name
sender[last_name] | string | no | Customer last name
title | string | no | Ticket title. Send it: without it the ticket is created with an empty title
content | string | yes | Ticket content (HTML is allowed and sanitized)
priority | string | no | Ticket priority: `normal` (default), `medium` or `critical`
custom_fields | object | no | Ticket custom field values, keyed by the custom field slug (the `cf_` slug, e.g. `cf_website_url`, see [Ticket Custom Fields](#ticket-custom-fields)). Unknown keys are ignored
attachments | array | no | Files to attach. Each item: `url` (publicly downloadable URL), `filename` and `contentType`. Only allowed file types are saved
isMarkDown | boolean | no | `true` if `content` is Markdown; it is converted to HTML

Errors: an unknown token or a missing/invalid `sender.email` returns HTTP `423` with `{"success": false, "data": {"message": "Invalid request."}}`. More than 200 requests per minute from one IP (or 120 per minute per webhook and IP) returns HTTP `429`. A blocked customer returns the error code `customer_inactive`.

## Email Piping Push

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/mail-piping/<mailbox_id>/push/<box_token>' \
--form 'payload="{\"subject\":\"Re: Refund request #118\",\"body_text\":\"<p>Thanks!</p>\",\"messageId\":\"<abc@mail.example.com>\",\"from\":{\"value\":[{\"name\":\"John Doe\",\"address\":\"john@example.com\"}]},\"attachments\":[]}"'
```

> The above command pushes an incoming email to the mailbox

```json
{
    "type": "new_response",
    "ticket_id": 118,
    "response_id": 942,
    "response": {
        "id": 942,
        "ticket_id": 118,
        "person_id": 7,
        "conversation_type": "response",
        "content": "<p>Thanks!</p>",
        "source": "email"
    },
    "customer": {
        "id": 7,
        "email": "john@example.com"
    }
}
```

This endpoint receives emails from the Fluent Support email piping server (and custom pipes) for an `email` type mailbox. Each email creates a new ticket (`"type": "new_ticket"`) or a reply to an existing ticket (`"type": "new_response"`), matched by the email threading headers, the ticket number in the subject, or a reply subject. The webhook URL is shown by [Get Email Piping Status](#get-email-piping-status). The method is not restricted (`ANY`).

Other responses: an email that was already processed returns `{"message": "skipped", "type": "confirmed"}`; a ticket that could not be created or a blocked customer returns `{"type": "confirmed", "message": "<error code>"}`; other failures return an error with `message`, `type: "error"` and `reason`. A wrong token returns "Token mismatch", and a payload larger than 5 MB returns HTTP `413`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`ANY https://yourdomain.com/wp-json/fluent-support/v2/mail-piping/<mailbox_id>/push/<box_token>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
payload | string | yes | The email as a JSON encoded string (keys below)
payload.from.value | array | yes, or `from_email` | Senders, e.g. `[{"name": "John Doe", "address": "john@example.com"}]`. The first one is used
payload.from_name / payload.from_email | string | no | Sender name and email, used when `from.value` is not sent
payload.subject | string | no | Email subject
payload.body_text | string | yes | Email body (HTML)
payload.messageId | string | no | The email's Message-ID
payload.inReplyTo | string | no | The In-Reply-To header, for threading
payload.references | string | no | The References header, for threading
payload.cc.value / payload.to.value | array | no | CC and To recipients (`[{"address": "..."}]`), saved as the ticket's CC list
payload.forwarded | object | no | `{"name": "...", "address": "..."}` original sender, honored only when the email was forwarded by the mailbox's own address
payload.attachments | array | no | Each item: `url`, `filename`, `contentType`
payload.isMarkDown | boolean | no | `true` if `body_text` is Markdown
payload.mime | string | no | Unique ID of the email, used to skip duplicates

## Telegram Bot Response

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/public/telegram_bot_response/<telegram_token>' \
--header 'Content-Type: application/json' \
--header 'X-Telegram-Bot-Api-Secret-Token: <secret>' \
--data-raw '{
    "message": {
        "from": { "id": 123456789 },
        "text": "Thanks, this is fixed now. [close]",
        "reply_to_message": { "text": "New ticket #118 ..." }
    }
}'
```

> The above command records an agent's reply sent from Telegram

```json
{
    "message": "Response has been successfully recorded",
    "status": true,
    "data": {
        "conversation_type": "response",
        "content": "Thanks, this is fixed now. ",
        "source": "telegram"
    }
}
```

This endpoint is the Telegram bot webhook. When an agent replies in Telegram to one of the bot's ticket notifications, the reply is added to the ticket; `[close]` in the text also closes the ticket. The sender is mapped to an agent by their Telegram ID, and the reply is saved only if that agent can access the ticket. Requires "reply from Telegram" to be enabled. Payloads that are ignored still answer `200` (with `"data": false`) so Telegram does not retry them. A wrong token or signature returns `403`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/public/telegram_bot_response/<telegram_token>`

## Slack Response

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/public/slack_response/<slack_token>' \
--header 'Content-Type: application/json' \
--header 'X-Slack-Request-Timestamp: <timestamp>' \
--header 'X-Slack-Signature: <signature>' \
--data-raw '{
    "type": "event_callback",
    "event": {
        "type": "message",
        "channel": "C0123456789",
        "thread_ts": "1730714400.000100",
        "user": "U0123456789",
        "client_msg_id": "3f1c...",
        "text": "#note Checked the logs, looks like a cache issue"
    }
}'
```

> The above command records an agent's reply sent from a Slack thread

```json
{
    "message": "received",
    "result": true
}
```

This endpoint is the Slack Events API request URL. It answers Slack's `url_verification` challenge (returning the `challenge` as plain text), and when an agent replies in the thread of a ticket notification in the configured channel, adds the reply to the ticket. `#note` or `#internal` in the text saves it as an internal note; `[close]` closes the ticket. The Slack user is mapped to an agent (or the fallback agent), who must be able to access the ticket. Requires "reply from Slack" to be enabled; ignored events return `"result": false`. An invalid signature or token returns `403`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/public/slack_response/<slack_token>`

## Twilio (WhatsApp) Response

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/public/twilio-response/<twilio_token>' \
--header 'X-Twilio-Signature: <signature>' \
--data-urlencode 'Body=##118## Your refund has been processed.' \
--data-urlencode 'From=whatsapp:+15551234567' \
--data-urlencode 'MessageSid=SM0123456789abcdef'
```

> The above command records an agent's reply sent from WhatsApp

```text
Response Added Successfully To Ticket No. 118
```

This endpoint is the Twilio WhatsApp incoming message webhook. An agent replies to a ticket by starting the WhatsApp message with `##TICKET_ID##`. The sender number is mapped to an agent, who must be able to access the ticket. The answer is plain text (shown to the agent in WhatsApp), e.g. "No ticket id declared to message body, declare ticket id like this: ##TICKET_ID_NUMBER##" or "Sorry No Ticket Found With Ticket No. 118". Requires "reply from WhatsApp" to be enabled. Requests must carry a valid Twilio signature and are limited to 60 per minute per IP.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/public/twilio-response/<twilio_token>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
Body | string | yes | Message text starting with `##TICKET_ID##`
From | string | yes | Sender number, e.g. `whatsapp:+15551234567`
MessageSid | string | no | Twilio message ID, used to skip duplicates

## Dropbox OAuth Callback

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/public/dropbox_auth?code=<auth_code>&state=<state>'
```

> The above command completes the Dropbox authorization and redirects to the settings page

```text
HTTP/1.1 302 Found
Location: https://yourdomain.com/wp-admin/admin.php?page=fluent-support#/settings/upload_integration
```

This endpoint is the OAuth redirect URL for the Dropbox file upload integration. It checks the `state` value against the one stored when the authorization started, exchanges the `code`, and redirects back to the upload integration settings (with `&error=invalid_state` if the state does not match).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/public/dropbox_auth`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
code | string | yes | Authorization code from Dropbox
state | string | yes | OAuth state: the user ID, a pipe separator, and a random value

## Google Drive OAuth Callback

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/public/google_auth?code=<auth_code>&state=<state>'
```

> The above command completes the Google Drive authorization and redirects to the settings page

```text
HTTP/1.1 302 Found
Location: https://yourdomain.com/wp-admin/admin.php?page=fluent-support#/settings/upload_integration
```

This endpoint is the OAuth redirect URL for the Google Drive file upload integration. It checks the `state` value, exchanges the `code` server side, and redirects back to the upload integration settings (with `&error=invalid_state` if the state does not match).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/public/google_auth`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
code | string | yes | Authorization code from Google
state | string | yes | OAuth state: the user ID, a pipe separator, and a random value

## Help Scout OAuth Callback

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/public/authorize?code=<auth_code>'
```

> The above command redirects to the Help Scout import screen with the code

```text
HTTP/1.1 302 Found
Location: https://yourdomain.com/wp-admin/admin.php?page=fluent-support#/help_scout?code=<auth_code>
```

This endpoint is the OAuth redirect URL for the Help Scout ticket importer. It only redirects to the Help Scout import screen in the admin, passing the `code` on.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/public/authorize`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
code | string | yes | Authorization code from Help Scout
