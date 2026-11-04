# Ticket Custom Fields

Custom fields add extra inputs to the ticket form. Their values are stored per ticket under the field's `slug`.

### Field definition

Key | Type | Description
--- | ---- | -----------
slug | text | Unique key of the field, always starting with `cf_` (e.g. `cf_website_url`). Generated from the label (max 20 characters) when not given. Keep the slug of an existing field: ticket values are stored under it.
type | text | One of the keys returned in `field_types`: `text`, `textarea`, `number`, `select-one`, `radio`, `checkbox`, `date-range`, plus integration types when the integration is active (e.g. `woo_orders`, `woo_products`, `edd_orders`, `fct_orders`, `learndash_courses`, `pmpro_levels`, `bb_groups`, `fcom_courses`, ...).
label | text | Label shown to customers. Required: a field without a label is dropped.
admin_label | text | Label shown to agents. Optional.
placeholder | text | Placeholder text. Optional.
options | array | Choices for `select-one`, `radio` and `checkbox` fields.
field_key | text | Optional extra key used by some integration field types.
required | text | `yes` or `no`.
admin_only | text | `yes` to hide the field from customers (agents only).
has_logics | text | `yes` to make a required field required only when its conditions match.
match_type | text | `any` (default) or `all`: how the conditions combine.
conditions | array | Conditions, each `{ "item_key": "...", "operator": "...", "value": "..." }`. `operator` is one of `=`, `!=`, `gt`, `lt`, `contains`, `not_contains`, `in`, `not_in` (`in`/`not_in` take a comma separated value).

## Get custom fields

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-custom-fields' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "fields": [
        {
            "slug": "cf_website_url",
            "type": "text",
            "label": "Website URL",
            "placeholder": "https://",
            "match_type": "any",
            "required": "yes",
            "admin_only": "no",
            "has_logics": "no"
        },
        {
            "slug": "cf_plan",
            "type": "select-one",
            "label": "Your Plan",
            "admin_label": "Plan",
            "options": ["Single", "Agency"],
            "match_type": "all",
            "conditions": [
                {
                    "item_key": "ticket_product_id",
                    "operator": "=",
                    "value": "Fluent Support"
                }
            ],
            "required": "yes",
            "admin_only": "no",
            "has_logics": "yes"
        }
    ],
    "field_types": {
        "text": {
            "type": "text",
            "label": "Single Line Text",
            "value_type": "string",
            "required": "no"
        },
        "textarea": {
            "type": "textarea",
            "label": "Multi Line Text",
            "value_type": "string",
            "required": "no"
        },
        "number": {
            "type": "number",
            "label": "Numeric Field",
            "value_type": "numeric",
            "required": "no"
        },
        "select-one": {
            "type": "select-one",
            "label": "Select Choice",
            "value_type": "string",
            "required": "no"
        },
        "radio": {
            "type": "radio",
            "label": "Radio Choice",
            "value_type": "string",
            "required": "no"
        },
        "checkbox": {
            "type": "checkbox",
            "label": "Checkboxes",
            "value_type": "array",
            "required": "no"
        },
        "date-range": {
            "type": "date-range",
            "label": "Date Range",
            "value_type": "array",
            "required": "no"
        }
    }
}
```

This endpoint returns the custom field definitions and the available field types.

<aside class="notice">This endpoint requires Fluent Support Pro and the settings permission.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket-custom-fields`

## Save custom fields

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-custom-fields' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "fields": "[{\"slug\":\"cf_website_url\",\"type\":\"text\",\"label\":\"Website URL\",\"placeholder\":\"https://\",\"required\":\"yes\"},{\"type\":\"checkbox\",\"label\":\"Browsers\",\"options\":[\"Chrome\",\"Firefox\",\"Safari\"]}]"
}'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Custom fields are successfully saved",
    "fields": [
        {
            "slug": "cf_website_url",
            "type": "text",
            "label": "Website URL",
            "placeholder": "https://",
            "match_type": "any",
            "required": "yes",
            "admin_only": "no",
            "has_logics": "no"
        },
        {
            "slug": "cf_browsers",
            "type": "checkbox",
            "label": "Browsers",
            "options": ["Chrome", "Firefox", "Safari"],
            "match_type": "any",
            "required": "no",
            "admin_only": "no",
            "has_logics": "no"
        }
    ]
}
```

This endpoint replaces the whole list of custom fields with the ones sent. Fields with an unknown type or no label are dropped, and unknown keys are ignored. Ticket values of fields that are removed are deleted. The saved definitions are returned.

<aside class="notice">This endpoint requires Fluent Support Pro and the settings permission.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/ticket-custom-fields`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
fields | text | yes | A JSON encoded array of [field definitions](#field-definition). This is the complete list: fields not included are removed.

## Update custom field values of a ticket

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-custom-fields/<ticket_id>/sync' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "custom_fields": {
        "cf_website_url": "https://example.com",
        "cf_browsers": ["Chrome", "Safari"]
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Custom Data has been updated",
    "custom_data": {
        "cf_website_url": "<a href=\"https://example.com\" target=\"_blank\">https://example.com</a>",
        "cf_browsers": ["Chrome", "Safari"]
    },
    "custom_data_rendered": {
        "cf_website_url": "<a href=\"https://example.com\" target=\"_blank\">https://example.com</a>",
        "cf_browsers": ["Chrome", "Safari"]
    }
}
```

This endpoint saves custom field values on a ticket. Values are keyed by field slug; unknown slugs are ignored and an empty value removes the stored value. `checkbox` and `date-range` values are arrays. `custom_data_rendered` shows integration field values (orders, courses, ...) rendered for display.

Customers set custom field values when they create a ticket from the customer portal, by sending them in `custom_data` keyed by slug.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/ticket-custom-fields/<ticket_id>/sync`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
custom_fields | object | yes | Field values keyed by field slug, e.g. `custom_fields[cf_website_url]=https://example.com`.
