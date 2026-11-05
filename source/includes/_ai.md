# AI Assistant

These endpoints use the AI provider configured in the AI settings of Fluent Support. If AI is disabled or no provider is configured, they return an error such as `AI features are disabled.` or `No AI provider configured.`. Only the customer-visible replies of a ticket (no internal notes or drafts) are sent to the AI provider.

## Get preset prompts

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ai/preset-prompts?type=createResponse' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
[
    {
        "label": "Request More Information",
        "text": "requestInfo",
        "description": "Ask the customer for the specific details we still need to resolve their issue."
    },
    {
        "label": "Acknowledge Issue",
        "text": "acknowledgeIssue",
        "description": "Acknowledge the customer's issue and tell them we are looking into it."
    },
    {
        "label": "Provide Solution",
        "text": "provideSolution",
        "description": "Give the customer the solution, with clear steps if they need to do something."
    },
    {
        "label": "Follow Up",
        "text": "followUp",
        "description": "Follow up to check whether the solution we provided resolved the issue."
    },
    {
        "label": "Close Ticket",
        "text": "closeTicket",
        "description": "Tell the customer the issue is resolved and the ticket will be closed, and that they can reply if they need anything else."
    }
]
```

This endpoint returns the built-in prompts shown in the AI menu of the reply editor. The `description` is the prompt text to send as `content` to [Generate a response](#generate-a-response-with-ai).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/ai/preset-prompts`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
type | text | no | `createResponse` for prompts that draft a reply, `modifyResponse` for prompts that rewrite selected text. Any other value, or no value, returns an empty list.

## Generate a response with AI

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ai/<ticket_id>/generate-response' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "content": "Ask the customer for the specific details we still need to resolve their issue.",
    "type": "response"
}'
```

> The above command returns the generated text as a JSON string:

```json
"Hi Sarah,\n\nCould you share the URL of the page where the form is not submitting, and a screenshot of any error you see? With that we can check it right away."
```

This endpoint drafts a reply to the ticket with AI, or rewrites a piece of text. With `type` `response` the AI writes a reply to the customer's latest message, using the ticket's conversation. With `type` `modifyResponse` it applies the prompt to `selectedText` and returns the edited text. Nothing is saved to the ticket.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/ai/<ticket_id>/generate-response`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
content | text | yes | The instruction (prompt) for the AI.
type | text | no | `response` (default) to draft a reply, or `modifyResponse` to rewrite `selectedText`.
selectedText | text | no | The text to rewrite. Used when `type` is `modifyResponse`.

## Get ticket summary

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ai/<ticket_id>/get-ticket-summary' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns the summary as a JSON string:

```json
"- Customer's contact form stopped sending emails.\n- SMTP plugin was reinstalled without success.\n- Agent asked for the SMTP log.\n- Waiting for the customer to send the log."
```

This endpoint returns a short AI summary of the ticket (at most 5 bullet points: the problem, what has been tried and what is still needed).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/ai/<ticket_id>/get-ticket-summary`

## Get ticket tone

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ai/<ticket_id>/get-ticket-tone' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns the tone as a JSON string:

```json
"Negative"
```

This endpoint classifies the customer's tone (sentiment) in the ticket, mostly from their latest message. The result is one word: `Positive`, `Neutral` or `Negative`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/ai/<ticket_id>/get-ticket-tone`
