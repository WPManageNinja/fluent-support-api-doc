# Remote Portal

Remote Portal lets customers use the support portal on a different WordPress site (the "main site") through the Fluent Support Connector plugin, while tickets stay on this (support) site.

There are two groups of endpoints:

* `settings/remote-portal*` are used by an administrator on this site (Settings > Remote Portal). They require the `fst_manage_settings` permission; enabling/disabling, creating a connection key and disconnecting also require a site administrator (`manage_options`).
* `remote-portal/*` are called by the Fluent Support Connector on the main site.

**How `remote-portal/*` requests are authorized**

1. An administrator turns Remote Portal on and generates a one-time connection key (`POST settings/remote-portal/connection-key`). The key is valid for 30 minutes and works once.
2. The administrator enters that key in the Connector on the main site, together with the username and a WordPress Application Password of the user the portal will act as. The Connector sends Basic auth with that Application Password on every request.
3. `POST remote-portal/connect` only needs Remote Portal to be enabled and a logged-in (authenticated) user; the controller then checks the connection key and that the user can act on behalf of customers (a site administrator, or an agent with the Sensitive Data, Manage Other Tickets and Manage Settings permissions).
4. Every other `remote-portal/*` request is allowed only while Remote Portal is enabled and only for the exact user who connected; another agent's Application Password is refused. When the connection is set in `wp-config.php` (`FLUENT_SUPPORT_PORTAL_BASE_URL`), any user who can act on behalf of customers passes, but `portal-url`, `disconnect` and `rotate-token` are refused (`rotate-token` is also refused when `FLUENT_SUPPORT_REMOTE_API_TOKEN` is set).

All requests are refused while Remote Portal is switched off or while the legacy Fluent Support Server plugin is active.

## Get Remote Portal Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the Remote Portal status

```json
{
    "settings": {
        "enabled": "yes",
        "remote_url": "https://mainsite.com",
        "portal_url": "https://mainsite.com/support",
        "connector_version": "1.0.0",
        "providers": ["woocommerce"],
        "connected_at": "2026-11-01 09:30:00",
        "last_inbound_at": "2026-11-04 10:00:00",
        "last_outbound_at": "2026-11-04 09:55:00",
        "last_outbound_ok": "yes",
        "has_token": true
    },
    "connected": true,
    "connected_user": {
        "id": 1,
        "display_name": "Admin",
        "user_login": "admin",
        "role": "admin",
        "mailboxes": "All mailboxes",
        "profile_url": "https://yourdomain.com/wp-admin/profile.php#application-passwords-section"
    },
    "constant_mode": false,
    "constants": [],
    "legacy_plugin": false,
    "support_url": "https://yourdomain.com",
    "app_passwords": true,
    "https": true,
    "key_expires_at": null,
    "connector_download_url": "https://fluentapi.wpmanageninja.com/addons/download/fluent-support-client/latest.zip",
    "can_manage": true
}
```

This endpoint returns the Remote Portal settings and connection status: the connected main site, the user it acts as, whether the connection is set by `wp-config.php` constants, and when a pending connection key expires.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal`

## Update Remote Portal Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command turns Remote Portal on or off

```json
{
    "message": "Settings saved",
    "settings": {
        "enabled": "yes"
    },
    "connected": false
}
```

This endpoint switches Remote Portal on or off. The response also contains the full status (same shape as [Get Remote Portal Settings](#get-remote-portal-settings)). Only a site administrator can change it (403 otherwise), and it is refused (422) when `FLUENT_SUPPORT_REMOTE_PORTAL` is defined in `wp-config.php`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
enabled | string | yes | `yes` to enable; any other value disables

## Create Connection Key

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal/connection-key' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command generates a one-time connection key

```json
{
    "connection_key": "fsc1.CONNECTION_KEY",
    "expires_at": 1793785200,
    "settings": {
        "enabled": "yes"
    },
    "connected": false
}
```

This endpoint generates a one-time connection key to paste into the Fluent Support Connector on the main site. The key (prefixed `fsc1.`) encodes this site's URL, REST URL and a secret; it expires after 30 minutes and can be used once. Generating a new key replaces the pending one. The response also contains the full status. Only a site administrator can create one; it is refused when Remote Portal is off or the connection is set in `wp-config.php`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal/connection-key`

## Disconnect Main Site (Settings)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal/disconnect' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command disconnects the main site

```json
{
    "message": "The main site has been disconnected. Revoke the Application Password of admin on their profile to stop its portal too.",
    "settings": {
        "enabled": "yes"
    },
    "connected": false
}
```

This endpoint removes the main site connection and any pending connection key. The main site still holds the Application Password, so revoke it on the user's profile to fully cut it off. Only a site administrator can disconnect; it is refused when the connection is set in `wp-config.php`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal/disconnect`

## Test Main Site Connection

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal/test' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command tests the connection to the main site

```json
{
    "message": "The main site answered with a valid token.",
    "settings": {
        "enabled": "yes"
    },
    "connected": true
}
```

This endpoint calls the main site's widget endpoint with the stored API token to verify the connection. On failure it returns a 422 error with the reason and the status.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-portal/test`

## Connect (Connector)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/connect' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command connects a main site

```json
{
    "success": true,
    "support_url": "https://yourdomain.com",
    "site_name": "My Support Site",
    "admin_portal_url": "https://yourdomain.com/wp-admin/admin.php?page=fluent-support/#/",
    "pro_version": "2.5.0",
    "mailboxes": [
        {
            "id": 1,
            "name": "Support",
            "is_default": "yes"
        }
    ],
    "default_mailbox_id": 1,
    "products": [
        {
            "id": 1,
            "title": "My Product"
        }
    ]
}
```

This is the Connector's first call. It checks the one-time connection key, that the authenticated user can act on behalf of customers, and the main site's URLs and API token, then saves the connection with the current user as the connected user. `mailboxes` only lists the mailboxes the connected user can access.

Errors: 401 when the key is invalid, expired or already used; 403 when the user cannot act for customers; 422 for an invalid address, a non-HTTPS main site (except on dev sites), a malformed API token, a token that differs from `FLUENT_SUPPORT_REMOTE_API_TOKEN`, or when the connection is set in `wp-config.php`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/connect`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
connection_secret | string | yes | The secret from the connection key
site_url | string | yes | The main site URL (must be HTTPS, except on dev sites)
widget_endpoint | string | yes | The main site's widget endpoint URL (same origin as `site_url`)
api_token | string | yes | Token this site sends on widget calls, 32-128 letters and numbers
portal_url | string | no | Customer portal page on the main site (ignored if not on the same host)
connector_version | string | no | Connector plugin version
providers | array | no | Integration providers active on the main site, e.g. `woocommerce`

## Rotate API Token (Connector)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/rotate-token' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command replaces the API token

```json
{
    "success": true
}
```

This endpoint replaces the API token this site sends on widget calls to the main site. Refused when the connection or token is set in `wp-config.php`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/rotate-token`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
api_token | string | yes | New token, 32-128 letters and numbers

## Ping (Connector)

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/ping' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command checks the connection

```json
{
    "success": true,
    "site_name": "My Support Site",
    "admin_portal_url": "https://yourdomain.com/wp-admin/admin.php?page=fluent-support/#/",
    "pro_version": "2.5.0",
    "mailboxes": [
        {
            "id": 1,
            "name": "Support",
            "is_default": "yes"
        }
    ],
    "default_mailbox_id": 1,
    "products": []
}
```

This is the Connector's health check. It also refreshes the mailbox and product lists, and stores the main site's providers and connector version when sent.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/ping`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
providers | array | no | Integration providers active on the main site
connector_version | string | no | Connector plugin version

## Get Portal Config (Connector)

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/portal-config' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the customer portal config

```json
{
    "portal_config": {
        "is_remote": true,
        "ticket_statuses": {
            "new": "New",
            "active": "Active",
            "closed": "Closed"
        },
        "can_view_private_ticket_number": false,
        "recaptcha": {
            "enabled": false
        },
        "support_products": [
            {
                "id": 1,
                "title": "My Product"
            }
        ],
        "product_field_required": false,
        "customer_ticket_priorities": {
            "normal": "Normal",
            "medium": "Medium",
            "critical": "Critical"
        },
        "fallback_image": "https://yourdomain.com/wp-content/plugins/fluent-support/assets/images/icons/file.svg",
        "has_file_upload": true,
        "has_rich_text_editor": true,
        "max_file_upload": 3,
        "agent_feedback_rating": "no",
        "upload_ticket_ref": "serial",
        "i18n": {}
    }
}
```

This endpoint returns the customer portal settings the Connector renders its portal with (statuses, products, priorities, upload limits and translated strings). It is the same for all customers. The values pass through the `fluent_support/customer_portal_vars` filter.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/portal-config`

## Update Portal URL (Connector)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/portal-url' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command updates the portal page URL on the main site

```json
{
    "success": true,
    "portal_url": "https://mainsite.com/support"
}
```

This endpoint updates where customers are sent for the portal. The URL must be on the main site's host (422 `The portal page must be on the main site.` otherwise). Refused when the connection is set in `wp-config.php`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/portal-url`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
portal_url | string | yes | The customer portal page URL on the main site

## Test Widgets (Connector)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/test-widgets' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command asks this site to call the main site's widget endpoint

```json
{
    "success": true
}
```

This endpoint makes this site call the main site's widget endpoint with the stored token, so the Connector can confirm the token works in that direction too. On failure it returns a 422 error with the reason.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/test-widgets`

## Disconnect (Connector)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/disconnect' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command disconnects from the main site

```json
{
    "success": true,
    "message": "Disconnected"
}
```

This endpoint lets the Connector remove its connection from this site. Refused when the connection is set in `wp-config.php`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/remote-portal/disconnect`
