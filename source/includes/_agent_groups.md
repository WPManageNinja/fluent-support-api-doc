# Agent Groups

Agent groups organize support staff into teams. One group can be marked as the default group, which new agents join automatically. All agent group endpoints require the `fst_sensitive_data` permission.

## Get All Agent Groups
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/agent-groups?per_page=10&page=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return all agent groups

```json
{
    "groups": {
        "total": 1,
        "per_page": 10,
        "current_page": 1,
        "last_page": 1,
        "next_page_url": null,
        "prev_page_url": null,
        "from": 1,
        "to": 1,
        "data": [
            {
                "id": 3,
                "title": "Support Team",
                "slug": "support-team",
                "description": "First line support",
                "settings": {
                    "is_default": true
                },
                "created_at": "2024-05-10 10:12:45",
                "updated_at": "2024-05-10 10:12:45",
                "agents_count": 4
            }
        ]
    }
}
```

This endpoint returns the agent groups, newest first, with the number of agents in each group.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/agent-groups`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Records per page
page | int | no | Page Number for Pagination, default 1
search | string | no | Search groups by title

## Create an Agent Group
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/agent-groups' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Support Team",
    "description": "First line support",
    "is_default": true,
    "agent_ids": [1, 2]
}'
```

> The above command will create a new agent group

```json
{
    "message": "Agent group has been created",
    "group": {
        "id": 3,
        "title": "Support Team",
        "slug": "support-team",
        "description": "First line support",
        "settings": {
            "is_default": true
        },
        "created_at": "2024-05-10 10:12:45",
        "updated_at": "2024-05-10 10:12:45"
    }
}
```

This endpoint creates a new agent group. When `is_default` is true, every other group stops being the default.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/agent-groups`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | text | yes | Group title
description | text | no | Group description
is_default | boolean | no | Make this the default group for new agents, default false
agent_ids[] | array | no | Agent IDs to add as members; IDs that are not agents are ignored

## Get an Agent Group
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/agent-groups/<group_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return a specific agent group

```json
{
    "group": {
        "id": 3,
        "title": "Support Team",
        "slug": "support-team",
        "description": "First line support",
        "settings": {
            "is_default": true
        },
        "created_at": "2024-05-10 10:12:45",
        "updated_at": "2024-05-10 10:12:45",
        "agent_ids": [1, 2]
    },
    "agents": [
        {
            "id": 1,
            "first_name": "John",
            "last_name": "Doe"
        },
        {
            "id": 2,
            "first_name": "Jane",
            "last_name": "Doe"
        }
    ]
}
```

This endpoint returns a single agent group with its member agent IDs, plus the list of all agents to choose from.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/agent-groups/<group_id>`

## Update an Agent Group
```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/agent-groups/<group_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Support Team",
    "agent_ids": [1, 2, 5]
}'
```

> The above command will update an agent group

```json
{
    "message": "Agent group has been updated",
    "group": {
        "id": 3,
        "title": "Support Team",
        "slug": "support-team",
        "description": "First line support",
        "settings": {
            "is_default": true
        },
        "created_at": "2024-05-10 10:12:45",
        "updated_at": "2024-05-12 08:30:00"
    }
}
```

This endpoint updates an agent group. `title` is always required; `description`, `is_default` and `agent_ids` are only changed when they are sent. Sending `agent_ids` replaces the group's members.

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/agent-groups/<group_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | text | yes | Group title
description | text | no | Group description
is_default | boolean | no | Make this the default group for new agents
agent_ids[] | array | no | Agent IDs; replaces the current members

## Delete an Agent Group
```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/agent-groups/<group_id>?fallback_group_id=4' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will delete an agent group

```json
{
    "message": "Agent group has been deleted and agents have been reassigned"
}
```

This endpoint deletes an agent group and moves its members to the fallback group.

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/agent-groups/<group_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
fallback_group_id | int | yes | An existing, different group that receives this group's agents
