# Ticket Form

## Get Ticket Form Config
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/pro/form-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will returns ticket form config

```json
{
    "settings": {
        "enable_docs": "yes",
        "docs_post_types": [
            "post",
            "course"
        ],
        "docs_post_categories": [
            3
        ],
        "post_limits": "5",
        "disable_rich_text": "no",
        "disabled_fields": [
            "product_services"
        ],
        "product_required_field": "no",
        "submitter_type": "allowed_user_roles",
        "allowed_user_roles": [],
        "field_labels": {
            "subject": "Subject",
            "ticket_details": "Ticket Details",
            "details_help": "Please provide details about your problem",
            "product_services": "Related Product/Service",
            "priority": "Priority",
            "btn_text": "Create Ticket",
            "submit_heading": "Submit a Support Ticket",
            "create_ticket_cta": "Create a New Ticket"
        }
    },
    "settings_fields": {
        "enable_docs": {
            "label": "Knowledge Base Suggestion",
            "type": "inline-checkbox",
            "checkbox_label": "Enable knowledge base suggestion on ticket creation form",
            "true_label": "yes",
            "false_label": "no"
        },
        "docs_post_types": {
            "type": "checkbox-group",
            "label": "Knowledge Base Post Types",
            "options": {
                "post": "post",
                "course": "course"
            },
            "inline_help": "Select the post types that you want to show articles from",
            "wrapper_class": "",
            "dependency": {
                "depends_on": "enable_docs",
                "operator": "=",
                "value": "yes"
            }
        },
        "docs_post_categories": {
            "type": "multi-select",
            "label": "Filter by Category",
            "options": [
                {
                    "id": 3,
                    "label": "Guides"
                },
                {
                    "id": 7,
                    "label": "— Getting Started"
                }
            ],
            "placeholder": "All Categories",
            "data_type": "number",
            "dependency": {
                "depends_on": "docs_post_types",
                "operator": "contains",
                "value": "post"
            }
        },
        "post_limits": {
            "type": "input-text",
            "data_type": "number",
            "label": "Suggested Articles Limit",
            "wrapper_class": "",
            "dependency": {
                "depends_on": "enable_docs",
                "operator": "=",
                "value": "yes"
            }
        },
        "disabled_fields": {
            "type": "checkbox-group",
            "label": "Disabled Default Fields",
            "wrapper_class": "",
            "options": {
                "file_upload": "File Upload",
                "priority": "Priority",
                "product_services": "Product & Services"
            },
            "inline_help": "Checked fields will not be available on the ticket creation form"
        },
        "product_required_field": {
            "label": "Product & Service Field Requirement",
            "type": "inline-checkbox",
            "checkbox_label": "Mark product and service fields as required.",
            "true_label": "yes",
            "wrapper_class": "",
            "false_label": "no",
            "dependency": {
                "depends_on": "disabled_fields",
                "operator": "not_contains",
                "value": "product_services"
            }
        },
        "disable_rich_text": {
            "label": "Rich Text Editor Option",
            "type": "inline-checkbox",
            "checkbox_label": "Disable Rich Text Editor for Frontend",
            "true_label": "yes",
            "wrapper_class": "",
            "false_label": "no"
        },
        "submitter_type": {
            "type": "input-radio",
            "label": "Who can access the customer portal?",
            "options": [
                {
                    "id": "logged_in_users",
                    "label": "Any logged-in users"
                },
                {
                    "id": "allowed_user_roles",
                    "label": "Only selected user roles"
                }
            ]
        },
        "allowed_user_roles": {
            "type": "checkbox-group",
            "label": "Select User Roles for Customer Portal",
            "options": {
                "author": "Author",
                "contributor": "Contributor",
                "customer": "Customer",
                "editor": "Editor",
                "subscriber": "Subscriber"
            },
            "dependency": {
                "depends_on": "submitter_type",
                "operator": "=",
                "value": "allowed_user_roles"
            }
        },
        "field_labels": {
            "label": "Form Labels Customization",
            "source_label": "Field",
            "new_label": "Input Label",
            "type": "object-tabular-input",
            "options": {
                "subject": "Subject heading",
                "ticket_details": "Form content heading",
                "details_help": "Content help message",
                "product_services": "Product/Service heading",
                "priority": "Priority heading",
                "btn_text": "Create ticket button text",
                "submit_heading": "Create ticket page heading",
                "create_ticket_cta": "Ticket Create Call to Action"
            }
        }
    }
}
```

This endpoint returns the ticket form config (`settings`) and the form field definitions (`settings_fields`). The fields are always returned. The WooCommerce account menu option moved to Global Settings (`ticket_link_portal`).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/pro/form-settings`

## Update Ticket Form Config

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/pro/form-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will update ticket form config

```json
{
    "message": "Settings have been updated"
}
```
This endpoint saves the ticket form config. The whole `settings` object is stored as sent, so send every key (as returned by the GET endpoint), not only the changed ones.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/pro/form-settings`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
settings[enable_docs] | string | no | `yes` / `no`, suggest knowledge base articles on the ticket form
settings[docs_post_types][] | array | no | Post types to suggest articles from
settings[docs_post_categories][] | array | no | Category IDs to limit `post` suggestions to. Only kept when `docs_post_types` contains `post`
settings[post_limits] | int | no | Number of suggested articles
settings[disable_rich_text] | string | no | `yes` to disable the rich text editor on the frontend
settings[disabled_fields][] | array | no | Default fields to hide: `file_upload`, `priority`, `product_services`
settings[product_required_field] | string | no | `yes` to make the product/service field required
settings[submitter_type] | string | no | `logged_in_users` or `allowed_user_roles`
settings[allowed_user_roles][] | array | no | User roles allowed to use the customer portal (when `submitter_type` is `allowed_user_roles`)
settings[field_labels][field_key] | string | yes | Field label overrides; keys: `subject`, `ticket_details`, `details_help`, `product_services`, `priority`, `btn_text`, `submit_heading`, `create_ticket_cta`
