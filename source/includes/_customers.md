# Customers

All customer endpoints require the `fst_sensitive_data` permission. Deleting customers (single or bulk) also requires a site administrator (`manage_options`) by default; this can be turned off with the `fluent_support/customer_delete_requires_admin` filter.

## Get all customers

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customers?per_page=10&page=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command get all customers.

```json
{
    "customers": {
        "total": 41,
        "per_page": 10,
        "current_page": 1,
        "last_page": 5,
        "next_page_url": "/wp-json/fluent-support/v2/customers?page=2",
        "prev_page_url": null,
        "from": 1,
        "to": 10,
        "data": [
            {
                "id": 84,
                "first_name": "John",
                "last_name": "Doe",
                "full_name": "John Doe",
                "email": "john@example.com",
                "photo": "https://www.gravatar.com/avatar/8dceee8bf8e7e7bf01cb9843120e7c0b?s=128",
                "avatar": null,
                "title": null,
                "status": "active",
                "user_id": null,
                "person_type": "customer",
                "city": null,
                "country": null,
                "created_at": "2021-12-04 16:10:54",
                "last_response_at": "2021-12-04 16:10:54",
                "total_tickets": 1,
                "open_tickets": 1,
                "total_responses": 0
            }
        ]
    }
}
```

Endpoint to get the list of available customers. Ticket counts only include tickets the current agent can access. When `smart_search=yes`, the response also contains a `search_meta` object describing how the search was interpreted.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customers`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Define how many customer you want to collect in every page, default it's 10.
page | int | no | Define the page number, default 1.
search | text | no | Search customer by name or email.
smart_search | text | no | Set `yes` to use the smart search syntax for `search`.
status | text | no | Filter customer by status `active`, `inactive` or `all`, default all.
has_open_tickets | text | no | Set `yes` to only return customers with open tickets.
sort_by | text | no | `created_at` (default), `last_response_at`, `first_name` or `id`. With `smart_search=yes` you can also use `relevance`.
sort_type | text | no | `ASC` or `DESC` (default).
import_contacts | text | no | When a search for an exact email finds nothing, the matching FluentCRM contact (or WordPress user with Pro) is imported as a customer. This only happens when neither `status` nor `has_open_tickets` is sent. Set `no` to disable this.

## Create a customer

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customers' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "jane@example.com",
    "first_name": "Jane",
    "last_name": "Doe",
    "status": "active"
}'

```

> The above command create a new customer.

```json
{
    "message": "Customer has been added",
    "customer": {
        "id": 85,
        "first_name": "Jane",
        "last_name": "Doe",
        "full_name": "Jane Doe",
        "email": "jane@example.com",
        "photo": "https://www.gravatar.com/avatar/9e26471d35a78862c17e467d87cddedf?s=128",
        "avatar": null,
        "title": null,
        "status": "active",
        "user_id": null,
        "person_type": "customer",
        "note": null,
        "address_line_1": null,
        "address_line_2": null,
        "city": null,
        "state": null,
        "zip": null,
        "country": null,
        "created_at": "2021-12-06 13:27:23",
        "updated_at": "2021-12-06 13:27:23",
        "last_response_at": null,
        "profile_edit_url": ""
    }
}
```

This endpoint will create a new customer. If a WordPress user exists with the same email, the customer is linked to that user automatically (`user_id` cannot be sent). The email must not belong to another customer.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customers`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
email | email | yes | Customer email, must be unique
first_name | text | yes | Customer first name
last_name | text | no | Customer last name
title | text | no | Customer job title
status | text | no | `active` or `inactive` (blocked)
note | text | no | Note for this customer
address_line_1 | text | no | Customer address field one
address_line_2 | text | no | Customer address field two
city | text | no | Customer city
state | text | no | Customer state
zip | text | no | Customer zip code
country | text | no | Customer country code

## Get a specific customer

```shell

curl --location -g --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customers/83?with[]=widgets&with[]=tickets&with[]=ticket_stats&with[]=fluentcrm_profile' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command get a specific customer.

```json
{
    "customer": {
        "id": 83,
        "first_name": "John",
        "last_name": "Doe",
        "full_name": "John Doe",
        "email": "john@example.com",
        "photo": "https://www.gravatar.com/avatar/8dceee8bf8e7e7bf01cb9843120e7c0b?s=128",
        "avatar": null,
        "title": null,
        "status": "active",
        "user_id": null,
        "person_type": "customer",
        "note": null,
        "address_line_1": null,
        "address_line_2": null,
        "city": null,
        "state": null,
        "zip": null,
        "country": null,
        "created_at": "2021-12-04 16:06:32",
        "updated_at": "2021-12-04 16:06:33",
        "last_response_at": "2021-12-04 16:06:33",
        "profile_edit_url": ""
    },
    "widgets": [],
    "tickets": [
        {
            "id": 143,
            "title": "Can I use Fluent Support with Fluent Forms",
            "status": "new",
            "customer_id": "83",
            "ticket_number": null,
            "serial_number": "12",
            "created_at": "2021-12-04 16:06:33",
            "updated_at": "2021-12-04 16:06:33"
        }
    ],
    "ticket_stats": {
        "new": 1
    },
    "fluentcrm_profile": false
}
```

Endpoint to get a specific customer by customer ID. Tickets and ticket stats only include tickets the current agent can access.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
with[] | array | no | Any of `widgets`, `tickets`, `ticket_stats` & `fluentcrm_profile`; the matching data is returned with the customer profile.

## Get customer form fields

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customers/customerField/83' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command get the customer form fields.

```json
{
    "customerField": {
        "basic_fields": {
            "first_name": {
                "label": "First Name",
                "data_type": "text",
                "placeholder": "First Name",
                "type": "input-text",
                "wrapper_class": "fs_half_field required"
            },
            "last_name": { "label": "Last Name", "data_type": "text", "placeholder": "Last Name", "type": "input-text", "wrapper_class": "fs_half_field" },
            "email": { "label": "Email", "data_type": "email", "placeholder": "Email", "type": "input-text", "wrapper_class": "fs_half_field required", "disabled": true },
            "title": { "label": "Job Title", "data_type": "text", "placeholder": "Job Title", "type": "input-text", "wrapper_class": "fs_half_field" },
            "note": { "label": "Note", "data_type": "textarea", "placeholder": "Note", "type": "input-text" },
            "status": {
                "label": "Status",
                "data_type": "text",
                "type": "input-radio",
                "options": {
                    "active": { "id": "active", "value": "active", "label": "Active" },
                    "inactive": { "id": "inactive", "value": "inactive", "label": "Blocked" }
                }
            },
            "status_html": {
                "dependency": { "depends_on": "status", "operator": "=", "value": "inactive" },
                "type": "html-viewer",
                "wrapper_class": "fs_warn_alert_wrapper",
                "html": "If you select the <b>Blocked</b> status, this customer will not be able to submit a ticket or any response."
            }
        },
        "address_fields": {
            "address_line_1": { "label": "Address Line 1", "data_type": "text", "placeholder": "Address Line 1", "type": "input-text", "wrapper_class": "fs_half_field" },
            "address_line_2": { "label": "Address Line 2", "data_type": "text", "placeholder": "Address Line 2", "type": "input-text", "wrapper_class": "fs_half_field" },
            "city": { "label": "City", "data_type": "text", "placeholder": "City", "type": "input-text", "wrapper_class": "fs_half_field" },
            "state": { "label": "State", "data_type": "text", "placeholder": "State", "type": "input-text", "wrapper_class": "fs_half_field" },
            "zip": { "label": "Zip Code", "data_type": "text", "placeholder": "Zip Code", "type": "input-text", "wrapper_class": "fs_half_field" },
            "country": { "label": "Country", "placeholder": "Country", "type": "country-selector", "wrapper_class": "fs_half_field" }
        },
        "loading": false
    }
}
```

This endpoint returns the field definitions used by the customer add/edit form. The email field is disabled when a customer ID is given. Use `0` as the customer ID for a new customer.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customers/customerField/<customer_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
user_id | int | no | The customer's linked WordPress user ID. When given, the custom registration fields are added to `address_fields`.

## Update a customer

```shell

curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/customers/83' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "john@example.com",
    "first_name": "John",
    "address_line_1": "353 South Milton Court"
}'

```

> The above command update a customer.


```json
{
    "message": "Customer has been updated",
    "customer": {
        "id": 83,
        "first_name": "John",
        "last_name": "Doe",
        "full_name": "John Doe",
        "email": "john@example.com",
        "photo": "https://www.gravatar.com/avatar/8dceee8bf8e7e7bf01cb9843120e7c0b?s=128",
        "avatar": null,
        "title": "",
        "status": "active",
        "user_id": "0",
        "person_type": "customer",
        "note": "",
        "address_line_1": "353 South Milton Court",
        "address_line_2": "33 Nobel Drive",
        "city": "168 Fabien Boulevard",
        "state": "435 Milton Avenue",
        "zip": "82363",
        "country": "",
        "created_at": "2021-12-04 16:06:32",
        "updated_at": "2021-12-06 13:27:23",
        "last_response_at": "2021-12-04 16:06:33",
        "profile_edit_url": ""
    }
}
```

This endpoint will update a specific customer by customer ID. Fields that are not sent are kept. Custom registration field values (if any are configured) can also be sent by their keys; they are stored on the linked WordPress user.

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
email | email | yes | Customer email, must not belong to another customer
first_name | text | yes | Customer first name
last_name | text | no | Customer last name
title | text | no | Customer title example: owner example co.
status | text | no | There are two values available for this field active & inactive
address_line_1 | text | no | Customer address field one
address_line_2 | text | no | Customer address field two
city | text | no | Customer city field
zip | text/int | no | Customer zip code field
state | text | no | Customer state field
country | text | no | Customer country field
note | text | no | Add note for this customer

## Delete a customer

```shell

curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command delete a customer.

```json

{
    "message": "Customer Deleted Successfully"
}

```

End point to delete a customer by Customer ID. All tickets of the customer are deleted too. By default only site administrators (`manage_options`) can delete customers; other users get a 403 error: "Only administrators can delete customers."

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>`

## Bulk delete customers

```shell

curl --location -g --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/customers/bulk-delete?customer_ids[]=83&customer_ids[]=84' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command delete multiple customers.

```json
{
    "message": "2 customers deleted successfully",
    "deleted_count": 2,
    "total_requested": 2
}
```

This endpoint deletes up to 100 customers at once, along with all of their tickets. Like single delete, it requires a site administrator (`manage_options`) by default; other users get a 403 error: "Only administrators can delete customers."

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/customers/bulk-delete`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
customer_ids[] | array | yes | Customer IDs to delete (1 to 100 IDs)

## Bulk update customer status

```shell

curl --location -g --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/customers/bulk-status?status=inactive&customer_ids[]=83&customer_ids[]=84' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command block multiple customers.

```json
{
    "message": "2 customers updated",
    "updated": 2
}
```

This endpoint blocks (`inactive`) or unblocks (`active`) up to 100 customers at once. `updated` counts only the customers whose status actually changed.

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/customers/bulk-status`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
customer_ids[] | array | yes | Customer IDs to update (1 to 100 IDs)
status | text | yes | `active` or `inactive`

## Upload customer profile image

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customers/profile_image/83' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--form 'file=@"/path/to/avatar.png"'

```

> The above command upload a new profile image for a customer.

```json
{
    "message": "Profile picture has been updated successfully",
    "image": "https://yourdomain.com/wp-content/uploads/fluent-support/customer_avatars/avatar.png",
    "customer": {
        "id": 83,
        "first_name": "John",
        "last_name": "Doe",
        "email": "john@example.com",
        "avatar": "https://yourdomain.com/wp-content/uploads/fluent-support/customer_avatars/avatar.png",
        "person_type": "customer",
        "status": "active",
        "full_name": "John Doe",
        "photo": "https://yourdomain.com/wp-content/uploads/fluent-support/customer_avatars/avatar.png"
    }
}
```

This endpoint uploads a profile image for a customer as a multipart form request. Allowed file types are jpeg, jpe, jpg and png.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customers/profile_image/<customer_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
file | file | yes | The image file (multipart form field `file`)

## Reset customer avatar

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customers/reset_avatar/83' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command reset a customer's avatar.

```json
{
    "message": "Customer avatar reset to gravatar default"
}
```

This endpoint removes the uploaded profile image of a customer, so the Gravatar image is used again.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customers/reset_avatar/<customer_id>`
