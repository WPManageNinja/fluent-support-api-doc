# Workflow

<aside class="notice">All workflow endpoints require Fluent Support Pro. They need the manage workflows permission (`fst_manage_workflows`); listing a workflow's actions and running it are also allowed with the run workflows permission (`fst_run_workflows`).</aside>

## Get Workflows
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/workflows' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above endpoint returns available workflows

```json
{
    "workflows": {
        "total": 2,
        "per_page": 10,
        "current_page": 1,
        "last_page": 1,
        "next_page_url": null,
        "prev_page_url": null,
        "from": 1,
        "to": 2,
        "data": [
            {
                "id": 5,
                "created_by": "1",
                "priority": "10",
                "title": "Add Internal Note",
                "trigger_key": "",
                "trigger_type": "manual",
                "settings": "",
                "status": "published",
                "last_ran_at": "0000-00-00 00:00:00",
                "created_at": "2021-12-07 15:14:29",
                "updated_at": "2021-12-07 15:15:48"
            },
            {
                "id": 4,
                "created_by": "1",
                "priority": "10",
                "title": "Assign Agent",
                "trigger_key": "fluent_support/ticket_created",
                "trigger_type": "automatic",
                "settings": {
                    "conditions": [
                        [
                            {
                                "data_key": "message.title",
                                "data_operator": "contains",
                                "data_value": "Fluent Support"
                            }
                        ]
                    ]
                },
                "status": "published",
                "last_ran_at": "0000-00-00 00:00:00",
                "created_at": "2021-12-07 15:08:37",
                "updated_at": "2021-12-07 15:09:25",
                "trigger_human_name": "On Ticket Creation"
            }
        ]
    }
}
```
This endpoint returns available workflows.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/workflows`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | How many workflows you want to show in page
page | int | no | Current page number
search | string | no | Search workflows by title

## Get a Specific Workflow
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>?with[]=action_fields&with[]=trigger_fields' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return a specific workflow

```json
{
    "actions": [
        {
            "id": 15,
            "title": "Add Internal Note",
            "action_name": "fs_action_create_note",
            "workflow_id": "5",
            "settings": {
                "response_body": "<p>We need to test this.</p>"
            },
            "created_at": "2021-12-07 15:15:48",
            "updated_at": "2021-12-07 15:15:48"
        }
    ],
    "action_fields": {
        "fs_action_create_response": {
            "title": "Add Response",
            "settings_defaults": {
                "response_body": ""
            },
            "fields": {
                "response_body": {
                    "type": "wp-editor",
                    "label": "Response Body"
                }
            }
        },
        "fs_action_assign_agent": {
            "title": "Assign Agent",
            "settings_defaults": {
                "agent_id": "",
                "skip_if_exist": "yes"
            },
            "fields": {
                "agent_id": {
                    "type": "agent-selectors",
                    "label": "Select Agent",
                    "extra_options": [
                        {
                            "id": "unassigned",
                            "title": "Unassigned"
                        }
                    ]
                },
                "skip_if_exist": {
                    "type": "inline-checkbox",
                    "true_label": "yes",
                    "false_label": "no",
                    "checkbox_label": "Skip if ticket already have a agent assigned"
                }
            }
        },
        "fs_action_create_note": {
            "title": "Add Internal Note",
            "settings_defaults": {
                "response_body": ""
            },
            "fields": {
                "response_body": {
                    "type": "wp-editor",
                    "label": "Note Body"
                }
            }
        },
        "fs_action_close_ticket": {
            "title": "Close Ticket",
            "settings_defaults": {
                "agent_id": "",
                "fallback_agent": ""
            },
            "fields": []
        },
        "fs_action_add_tags": {
            "title": "Add Tag(s)",
            "settings_defaults": {
                "tag_ids": []
            },
            "fields": {
                "tag_ids": {
                    "is_multiple": true,
                    "label": "Select Tags",
                    "type": "tag-selectors",
                    "placeholder": "Select Tags"
                }
            }
        },
        "fs_action_remove_tags": {
            "title": "Remove Tag(s)",
            "settings_defaults": {
                "tag_ids": []
            },
            "fields": {
                "tag_ids": {
                    "is_multiple": true,
                    "label": "Select Tags",
                    "type": "tag-selectors",
                    "placeholder": "Select Tags"
                }
            }
        },
        "fs_delete_ticket": {
            "title": "Delete Ticket",
            "settings_defaults": {
                "ticket_delete_html": ""
            },
            "fields": {
                "ticket_delete_html": {
                    "type": "html-viewer",
                    "html": "<p><br />Ticket will be deleted permanently and no further action will run</p>"
                }
            }
        },
        "fs_block_customer": {
            "title": "Block Ticket Submitter (Customer)",
            "settings_defaults": {
                "customer_block_html": ""
            },
            "fields": {
                "customer_block_html": {
                    "type": "html-viewer",
                    "html": "<p><br />Customer will be blocked and can not create new ticket or access to previous tickets</p>"
                }
            }
        }
    },
    "trigger_fields": {
        "triggers": {
            "fluent_support/ticket_created": {
                "title": "On Ticket Creation",
                "description": "This workflow will be initiated on when a new ticket has been submitted by customer",
                "supported_conditions": [
                    "customer.first_name",
                    "customer.last_name",
                    "customer.email",
                    "message.title",
                    "message.content",
                    "message.attachments",
                    "ticket.client_priority",
                    "ticket.mailbox_id",
                    "message.added_time_range",
                    "message.added_date_range",
                    "ticket.product_id"
                ]
            },
            "fluent_support/response_added_by_customer": {
                "title": "On Customer Response",
                "description": "Workflow will be initiated when customer add a response to an existing ticket",
                "supported_conditions": [
                    "customer.first_name",
                    "customer.last_name",
                    "customer.email",
                    "message.content",
                    "message.attachments",
                    "ticket.agent_id",
                    "ticket.mailbox_id",
                    "message.added_time_range",
                    "message.added_date_range",
                    "ticket.product_id"
                ]
            }
        },
        "conditions": {
            "customer.first_name": {
                "title": "Customer First Name",
                "data_type": "string",
                "group": "Customer"
            },
            "customer.last_name": {
                "title": "Customer Last Name",
                "data_type": "string",
                "group": "Customer"
            },
            "customer.email": {
                "title": "Customer Email",
                "data_type": "string",
                "group": "Customer"
            },
            "message.title": {
                "title": "Ticket Title",
                "data_type": "string",
                "group": "Message"
            },
            "message.content": {
                "title": "Message Content",
                "data_type": "string",
                "group": "Message"
            },
            "message.attachments": {
                "title": "Attachments",
                "data_type": "yes_no",
                "default": "yes",
                "options": {
                    "yes": "Has an attachment",
                    "no": "Does not have an attachment"
                },
                "group": "Message"
            },
            "message.added_time_range": {
                "title": "Added Time Range",
                "data_type": "time_range",
                "group": "Message"
            },
            "message.added_date_range": {
                "title": "Added Date Range",
                "data_type": "date_range",
                "group": "Message"
            },
            "ticket.client_priority": {
                "title": "Ticket Priority (Client)",
                "data_type": "single_dropdown",
                "options": {
                    "normal": "Normal",
                    "medium": "Medium",
                    "critical": "Critical"
                },
                "group": "Ticket"
            },
            "ticket.product_id": {
                "title": "Selected Product",
                "data_type": "single_dropdown",
                "options": {
                    "1": "Fluent Support",
                    "2": "Fluent Forms"
                },
                "group": "Ticket"
            }
        }
    },
    "workflow": {
        "id": 5,
        "created_by": "1",
        "priority": "10",
        "title": "Add Internal Note",
        "trigger_key": "",
        "trigger_type": "manual",
        "settings": "",
        "status": "published",
        "last_ran_at": "0000-00-00 00:00:00",
        "created_at": "2021-12-07 15:14:29",
        "updated_at": "2021-12-07 15:15:48"
    }
}
```

This endpoint will return a specific workflow with its ordered actions. `action_fields` and `trigger_fields` are only included when requested with `with[]`; without it the response has only `actions` and `workflow`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>?with[]=action_fields&with[]=trigger_fields`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
with[] | array | no | Extra data to include: `action_fields` (the available actions and their fields) and/or `trigger_fields` (the triggers and conditions)

## Create a New Workflow
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/workflows' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Close Ticket on Thank You",
    "trigger_type": "automatic",
    "trigger_key": "fluent_support/response_added_by_customer"
}'
```

> The above command will create a new workflow

```json
{
    "workflow": {
        "title": "Close Ticket on Thank You",
        "trigger_type": "automatic",
        "trigger_key": "fluent_support/response_added_by_customer",
        "created_by": 1,
        "status": "draft",
        "updated_at": "2026-11-04 18:11:08",
        "created_at": "2026-11-04 18:11:08",
        "id": 7
    },
    "message": "Workflow has been created"
}
```

This endpoint creates a new workflow as a `draft` owned by the current user. Add conditions and actions, and publish it, with the [Update a Workflow](#update-a-workflow) endpoint.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/workflows`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | string | yes | Workflow title. Must be unique
trigger_type | string | no | `manual` or `automatic`
trigger_key | string | no | The trigger hook for automatic workflows, e.g. `fluent_support/ticket_created` or `fluent_support/response_added_by_customer`

## Update a Workflow

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "workflow": {
        "title": "Close Ticket",
        "trigger_type": "automatic",
        "trigger_key": "fluent_support/response_added_by_customer",
        "status": "published",
        "settings": {
            "conditions": [
                [
                    {
                        "data_key": "message.content",
                        "data_operator": "contains",
                        "data_value": "Thank You"
                    }
                ]
            ]
        }
    },
    "actions": [
        {
            "title": "Close Ticket",
            "action_name": "fs_action_close_ticket",
            "settings": {
                "agent_id": "ticket_agent_id",
                "fallback_agent": "1"
            }
        }
    ]
}'
```
> The above command will update a specific workflow by id

```json
{
    "message": "Workflow has been updated",
    "workflow": {
        "id": 6,
        "created_by": "1",
        "title": "Close Ticket",
        "trigger_key": "fluent_support/response_added_by_customer",
        "trigger_type": "automatic",
        "settings": {
            "conditions": [
                [
                    {
                        "data_key": "message.content",
                        "data_operator": "contains",
                        "data_value": "Thank You"
                    }
                ]
            ]
        },
        "status": "published",
        "last_ran_at": null,
        "created_at": "2021-12-07 17:54:15",
        "updated_at": "2021-12-07 17:54:44"
    },
    "actions": [
        {
            "id": 17,
            "title": "Close Ticket",
            "action_name": "fs_action_close_ticket",
            "workflow_id": "6",
            "settings": {
                "agent_id": "ticket_agent_id",
                "fallback_agent": "1"
            },
            "created_at": "2021-12-07 17:54:44",
            "updated_at": "2021-12-07 17:54:44"
        }
    ]
}
```

This endpoint updates a workflow and saves its actions. The actions are kept in the order sent; an action with the `id` of one of this workflow's actions is updated, any other entry creates a new action, and existing actions left out of `actions` are deleted.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
workflow[title] | string | yes | Workflow title. Must be unique
workflow[trigger_type] | string | no | `manual` or `automatic`
workflow[trigger_key] | string | no | Trigger hook name for automatic workflows
workflow[status] | string | no | `draft` or `published`
workflow[settings][conditions][0][0][data_key] | string | no | Condition data key (see `trigger_fields.conditions` of [Get a Specific Workflow](#get-a-specific-workflow))
workflow[settings][conditions][0][0][data_operator] | string | no | Condition operator, e.g. `contains`
workflow[settings][conditions][0][0][data_value] | mixed | no | Condition value
actions[0][id] | int | no | ID of an existing action of this workflow to update
actions[0][title] | string | no | Action title
actions[0][action_name] | string | yes | Action key, e.g. `fs_action_close_ticket` (see `action_fields`)
actions[0][settings] | object | no | Action settings (keys from the action's `settings_defaults`)

## Delete a Workflow

```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will delete a specific workflow by id

```json
{
    "message": "Selected workflow has been deleted"
}
```

This endpoint will delete a specific workflow by id, with its actions.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>`

## Get Manual Workflow Options

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/options' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the published manual workflows

```json
{
    "option_key": "manual_workflows",
    "options": [
        {
            "id": 5,
            "title": "Add Internal Note"
        }
    ]
}
```

This endpoint returns the published workflows with the `manual` trigger type, used for the "Run workflow" menus on tickets.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/workflows/options`

## Search Commerce Options

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/commerce-options?provider=<provider>&type=product&search=pro' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command searches the products of a commerce provider

```json
{
    "options": [
        {
            "id": "125",
            "title": "Pro Plan"
        }
    ]
}
```

This endpoint searches the products (or variations) of a commerce integration for the commerce conditions of the workflow builder. It returns at most 50 options. When `ids` is given, those IDs are resolved back to their labels instead of searching. An unknown `provider` returns an empty `options` list.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/workflows/commerce-options`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
provider | string | yes | The commerce provider key (an active commerce integration, e.g. FluentCart or Easy Digital Downloads)
type | string | no | `variation` to search variations; anything else searches products
search | string | no | Search text
ids | string | no | Comma separated IDs to resolve (max 50). Overrides `search`

## Save Action Sequence

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/action-sequence' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "workflow_id": 7,
    "action_sequence": [22, 20, 21]
}'
```

> The above command saves the order of a workflow's actions

```json
{
    "message": "Action sequence has been updated"
}
```

This endpoint saves the order in which the actions of a workflow run.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/workflows/action-sequence`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
workflow_id | int | yes | Workflow ID
action_sequence | array | yes | Action IDs in the order they should run

## Duplicate a Workflow

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/duplicate/<workflow_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command duplicates a workflow

```json
{
    "message": "Selected workflow has been duplicated"
}
```

This endpoint copies a workflow, with its actions in the same order, as a `draft` titled "&lt;title&gt; - Copy" (then "- Copy (1)", "- Copy (2)", ...).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/workflows/duplicate/<workflow_id>`

## Get Workflow Actions

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>/actions' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the actions of a workflow

```json
{
    "actions": [
        {
            "id": 15,
            "title": "Add Internal Note",
            "action_name": "fs_action_create_note"
        }
    ]
}
```

This endpoint returns the actions (ID, title and action name) of a workflow. Agents with either the manage workflows or the run workflows permission can use it.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>/actions`

## Get Workflow Runs

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>/runs?limit=20' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the latest runs of a workflow

```json
{
    "runs": [
        {
            "id": 812,
            "failed": false,
            "description": "Workflow <a class=\"fs_link_trans\" href=\"#edit_workflow\">Add Internal Note</a> ran on <a class=\"fs_link_trans fs_tk\" href=\"#view_ticket\" data-ticket-id=\"118\">Ticket: Refund request</a> (Manual run)",
            "created_at": "2026-11-04 09:30:12"
        }
    ]
}
```

This endpoint returns the latest runs of a workflow from the activity log, newest first. Only runs on tickets the current agent can see are returned. `failed` is `true` when one or more actions failed; the failure reasons are appended to `description`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>/runs`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
limit | int | no | Number of runs to return, 1 to 100. Default 20

## Run a Workflow

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>/run' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ticket_ids": [118, 119]
}'
```

> The above command runs a manual workflow on tickets

```json
{
    "message": "Selected workflow actions have been successfully applied"
}
```

This endpoint runs a published `manual` workflow on the given tickets. Only tickets the current agent can access are processed. If some actions fail, the response is still successful but has `"has_failures": true` and the message "Some workflow actions could not be applied. Please check the error log for details." Agents with either the manage workflows or the run workflows permission can use it.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>/run`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket_ids | array | yes | Ticket IDs to run the workflow on (max 200)
