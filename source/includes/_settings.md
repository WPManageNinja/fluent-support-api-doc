# Global Settings

All `settings/*` endpoints require the `fst_manage_settings` permission (site administrators always have it).

## Get Global Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings?settings_key=global_business_settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return global business settings

```json
{
    "settings": {
        "portal_page_id": "547",
        "ticket_link_portal": "default",
        "ticket_link_portal_migrated": "yes",
        "login_message": "<p>Please login or create an account to access the Customer Support Portal</p> [fluent_support_auth]",
        "disable_public_ticket": "no",
        "accepted_file_types": [
            "images",
            "csv",
            "documents",
            "zip",
            "json",
            "text"
        ],
        "max_file_size": "2",
        "max_file_upload": "3",
        "del_files_on_close": "no",
        "enable_admin_bar_summary": "no",
        "internal_notifications_enabled": "no",
        "enable_draft_mode": "no",
        "agent_feedback_rating": "no",
        "keyboard_shortcuts": "yes",
        "enable_min_serial_number": "no",
        "ticket_prefix": "",
        "min_serial_number": 1,
        "enable_fluent_booking_integration": "no",
        "report_snapshot_retention": "15"
    },
    "fields": {
        "ticket_link_portal": {
            "type": "input-radio",
            "wrapper_class": "fs_portal_destination_options",
            "options": [
                {
                    "id": "default",
                    "label": "Default Portal Page"
                }
            ]
        },
        "portal_page_id": {
            "type": "input-options",
            "label": "Select a Page",
            "options": [
                {
                    "id": 547,
                    "title": "Support Portal"
                }
            ],
            "dependency": {
                "depends_on": "ticket_link_portal",
                "operator": "=",
                "value": "default"
            }
        },
        "max_file_size": {
            "wrapper_class": "fs_settings_half_field",
            "type": "input-text",
            "data_type": "number",
            "label": "Max File Size (in MegaByte)"
        }
    }
}
```

This endpoint returns a settings page with its values (`settings`) and the form field definitions (`fields`). The `fields` object is always returned and is shortened above.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
settings_key | string | yes | `global_business_settings` for the global business settings, or `_internal_notification_settings` for the in-app (internal) notification settings. Any other value returns a 404 error

## Update Global Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will update the settings

```json
{
    "message": "Settings has been updated"
}
```

This endpoint saves one settings page. Values are sanitized as plain text, except `login_message` which allows HTML. An unknown `settings_key` returns the error `Unknown settings.`

The whole settings object is replaced, not merged. Send every key as returned by [Get Global Settings](#get-global-settings), not only the changed ones. Omitted keys revert to their defaults, and a missing `accepted_file_types` is saved as an empty list (which disables all file types). The same applies to `_internal_notification_settings`.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
settings_key | string | yes | `global_business_settings`, `_internal_notification_settings` or `_fluentcrm_intergration_settings` (FluentCRM integration)
settings | object | yes | The settings to save. The keys depend on `settings_key` (see below)

**`global_business_settings` keys**

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
settings[ticket_link_portal] | string | no | Where ticket links send customers: `default`, `fluent_cart`, `woocommerce` or `fluent_community`
settings[portal_page_id] | int | no | The page with the `[fluent_support_portal]` shortcode (used when `ticket_link_portal` is `default`)
settings[login_message] | string (HTML) | no | Message shown to logged-out users
settings[disable_public_ticket] | string | no | `yes` to allow only logged-in users to reply, `no` to allow signed links
settings[accepted_file_types][] | array | no | Any of `images`, `csv`, `documents`, `zip`, `json`, `text`
settings[max_file_size] | int | no | Maximum file size in megabytes
settings[max_file_upload] | int | no | Maximum number of files per upload
settings[del_files_on_close] | string | no | `yes` to delete a ticket's attachments when it is closed
settings[enable_admin_bar_summary] | string | no | `yes` / `no`, show a ticket summary in the WordPress admin bar
settings[internal_notifications_enabled] | string | no | `yes` / `no`, enable in-app notifications for agents
settings[enable_draft_mode] | string | no | `yes` / `no`, keep reply drafts
settings[agent_feedback_rating] | string | no | `yes` / `no`, let customers rate agent replies
settings[custom_registration_form_field][] | array | no | Extra fields for the portal registration form
settings[enable_two_fa] | string | no | `yes` / `no`, two-factor authentication on the portal login
settings[keyboard_shortcuts] | string | no | `yes` / `no`
settings[enable_min_serial_number] | string | no | `yes` to use a minimum ticket number and prefix
settings[min_serial_number] | int | no | Minimum ticket number
settings[ticket_prefix] | string | no | Ticket number prefix
settings[report_snapshot_retention] | string | no | Days to keep report snapshots, e.g. `3`, `7`, `15`

**`_internal_notification_settings` keys**: `assignment_notifications`, `agent_reply_notifications`, `customer_reply_notifications`, `workflow_notifications`, `status_notifications`, `self_notifications` (each `yes` / `no`). Whether in-app notifications are on is set by `internal_notifications_enabled` in `global_business_settings`; an `enabled` key sent here has no effect.

**`_fluentcrm_intergration_settings` keys**: `enabled` (`yes` / `no`), `default_status` (`subscribed` or `pending`), `assigned_list` (FluentCRM list ID), `assigned_tags` (array of FluentCRM tag IDs).

## Get Enabled Chat Integrations

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/integration-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the keys of the enabled chat integrations

```json
[
    "slack_settings",
    "telegram_settings"
]
```

This endpoint returns the keys of the notification (chat) integrations whose status is switched on. The agent profile form uses it to show the matching fields (e.g. Telegram chat ID).

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/integration-settings`

## Get Notification Integration Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/integration?integration_key=slack_settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the settings of a notification integration

```json
{
    "settings": {
        "bot_token": "",
        "channel": "",
        "channel_id": "",
        "signing_secret": "",
        "status": "no",
        "ticket_assigned": "no",
        "reply_from_slack": "no",
        "fallback_agent_id": ""
    },
    "fields": {}
}
```

This endpoint returns the saved settings and form fields of a notification integration. Integrations are registered by add-ons; Fluent Support Pro registers `slack_settings`, `telegram_settings`, `discord_settings` and `twilio_settings` (WhatsApp). An unregistered key returns a 404 error.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/integration`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
integration_key | string | yes | The integration key, e.g. `slack_settings`, `telegram_settings`, `discord_settings`, `twilio_settings`

## Update Notification Integration Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/integration' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command saves the settings of a notification integration

```json
{
    "message": "Settings has been updated",
    "settings": {
        "bot_token": "SLACK_BOT_TOKEN",
        "channel": "support",
        "status": "yes"
    }
}
```

This endpoint saves a notification integration's settings. All values are sanitized as plain text. The integration may refuse the save (e.g. `Please fill in the required credentials.` when a required credential is missing) or adjust what it stores. An unregistered key returns `Settings failed to save`.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/integration`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
integration_key | string | yes | The integration key, e.g. `slack_settings`
settings | object | yes | The integration's settings, as returned by the GET endpoint

## Get Pages

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/pages' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the published WordPress pages

```json
{
    "pages": [
        {
            "id": 547,
            "title": "Support Portal"
        },
        {
            "id": 12,
            "title": "Contact"
        }
    ]
}
```

This endpoint returns all published WordPress pages, newest first. It is used to pick the customer portal page.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/pages`

## Setup Portal (Onboarding)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/setup' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command runs the onboarding wizard's first step

```json
{
    "mailbox": {
        "id": 1,
        "name": "Support",
        "email": "support@example.com",
        "box_type": "web",
        "is_default": "yes"
    },
    "global_settings": {
        "portal_page_id": 547,
        "login_message": "<p>Please login or create an account to access the Customer Support Portal</p> [fluent_support_auth]"
    },
    "mailboxes": [
        {
            "id": 1,
            "name": "Support",
            "settings": {
                "admin_email_address": "support@example.com"
            }
        }
    ],
    "has_fluentform": false
}
```

This endpoint is used by the onboarding wizard. It creates the first (default) mailbox when no mailbox exists yet, and sets the customer portal page, either an existing page or a new "Support Portal" page with the `[fluent_support_portal]` shortcode. If a mailbox already exists, the first one is returned and no new one is created.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/setup`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
mailbox[name] | string | yes | Mailbox name
mailbox[email] | string | yes | Mailbox email address
mailbox[box_type] | string | yes | Mailbox type, `web` or `email`
global_settings[create_portal_page] | string | no | `yes` to create a new portal page
global_settings[portal_page_id] | int | yes, unless `create_portal_page` is `yes` | ID of an existing page to use as the portal

## Finish Setup Installation (Onboarding)

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/setup-installation' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command completes the onboarding wizard

```json
{
    "message": "Installation has been completed"
}
```

This endpoint is the onboarding wizard's last step. It can install and activate Fluent Forms (only if the user can install plugins), turn on sharing of essential data, and subscribe an email address to the newsletter.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/setup-installation`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
install_fluentform | string | no | `yes` to install and activate Fluent Forms
share_essentials | string | no | `yes` to share essential (non-sensitive) usage data
optin_email | string | no | Email address to subscribe to the newsletter

## Get reCAPTCHA Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/recaptcha-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the reCAPTCHA settings

```json
{
    "reCaptcha_version": "recaptcha_v2",
    "siteKey": "RECAPTCHA_SITE_KEY",
    "secretKey": "RECAPTCHA_SECRET_KEY",
    "formContainingReCaptcha": {
        "login_form": "yes",
        "signup_form": "yes",
        "ticket_form": "no"
    },
    "is_enabled": "true"
}
```

This endpoint returns the saved reCAPTCHA settings, or an empty array when none are saved.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/recaptcha-settings`

## Save reCAPTCHA Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/recaptcha-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command saves the reCAPTCHA settings

```json
{
    "message": "Your reCAPTCHA settings added successfully."
}
```

This endpoint saves the reCAPTCHA keys. The first save must include a `captchaResponse` token that passes verification with the given keys (otherwise `Please verify reCAPTCHA before saving.`). Later saves return `Your reCAPTCHA settings updated successfully.` Send `reCaptcha=clear-reCaptcha-settings` to delete the settings (`Your reCAPTCHA settings deleted successfully.`).

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/recaptcha-settings`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
reCaptcha | object or string | yes | The settings object below, or the string `clear-reCaptcha-settings` to delete the settings
reCaptcha[reCaptchaVersion] | string | yes | `recaptcha_v2` or `recaptcha_v3`
reCaptcha[siteKey] | string | yes | reCAPTCHA site key
reCaptcha[secretKey] | string | yes | reCAPTCHA secret key
reCaptcha[formContainingReCaptcha] | object | no | Forms to protect: `login_form`, `signup_form`, `ticket_form` (each `yes` / `no`)
reCaptcha[reCaptchaEnabled] | string | no | Whether reCAPTCHA is enabled
reCaptcha[captchaResponse] | string | yes on first save | A reCAPTCHA response token used to verify the keys

## Get Integration Statuses

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/integration-statuses' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the status of every supported integration

```json
{
    "connections": {
        "woocommerce": {
            "title": "WooCommerce",
            "logo": "https://yourdomain.com/wp-content/plugins/fluent-support/assets/images/icons/integrations/woocommerce.png",
            "is_integrated": true,
            "description": "The most popular e-commerce platform for WordPress",
            "doc_url": "https://docs.fluentsupport.com/woocommerce-integration"
        },
        "slack": {
            "title": "Slack",
            "logo": "https://yourdomain.com/wp-content/plugins/fluent-support/assets/images/icons/integrations/slack.png",
            "is_integrated": false,
            "description": "Business communication platform designed to scale",
            "doc_url": "https://docs.fluentsupport.com/managing-tickets-using-slack"
        }
    }
}
```

This endpoint returns every supported integration (WooCommerce, FluentCart, LifterLMS, Slack, Telegram, Discord, WhatsApp, FluentCRM, Fluent Forms, Google Drive, Dropbox, reCAPTCHA, and more) and whether it is active/connected on this site. The list is shortened above.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/integration-statuses`

## Get FluentCRM Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/fluentcrm-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the FluentCRM integration settings

```json
{
    "is_installed": true,
    "settings": {
        "enabled": "yes",
        "default_status": "subscribed",
        "assigned_list": 2,
        "assigned_tags": [1, 4]
    },
    "settings_fields": {
        "enabled": {
            "type": "inline-checkbox",
            "true_label": "yes",
            "false_label": "no",
            "checkbox_label": "Enable FluentCRM Integration"
        },
        "default_status": {
            "type": "input-radio",
            "label": "Default status for new contacts",
            "options": [
                {"id": "subscribed", "label": "Subscribed"},
                {"id": "pending", "label": "Pending"}
            ]
        },
        "assigned_list": {
            "type": "input-options",
            "label": "Add to FluentCRM list (optional)",
            "options": [{"id": 2, "title": "Customers"}]
        },
        "assigned_tags": {
            "type": "input-options",
            "multiple": true,
            "label": "Add to Tags",
            "options": [{"id": 1, "title": "Support"}]
        }
    },
    "fluentcrm_logo": "https://yourdomain.com/wp-content/plugins/fluent-support/assets/images/fluentcrm-logo.svg"
}
```

This endpoint returns the FluentCRM integration settings with the lists and tags to choose from. When FluentCRM is not installed it returns only `is_installed: false` and `fluentcrm_logo`. Save these settings with [Update Global Settings](#update-global-settings) and `settings_key=_fluentcrm_intergration_settings`.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/fluentcrm-settings`

## Install FluentCRM

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/install-fluentcrm' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command installs and activates FluentCRM

```json
{
    "is_installed": true,
    "message": "FluentCRM plugin has been installed and activated successfully"
}
```

This endpoint installs and activates FluentCRM from wordpress.org (if it is not already active). The user needs the `install_plugins` capability.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/install-fluentcrm`

## Get File Storage Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-upload-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the file storage drivers

```json
{
    "drivers": {
        "local": {
            "title": "Default WordPress Storage",
            "is_disabled": false,
            "is_configured": true,
            "icon": "https://yourdomain.com/wp-content/plugins/fluent-support/assets/images/icons/folder.svg",
            "description": "Upload and store the files to your WordPress File System Storage."
        },
        "amazon_s3": {
            "meta_key": "amazon_s3_settings",
            "title": "Amazon S3",
            "has_config": true,
            "is_configured": false,
            "require_pro": false,
            "upgrade_url": "https://fluentsupport.com/pricing/",
            "description": "Upload and store the files to Amazon S3 cloud storage.",
            "icon": "https://yourdomain.com/wp-content/plugins/fluent-support/assets/images/icons/amazon-s3.svg",
            "health": {}
        }
    },
    "enabled_driver": "local",
    "async_upload": "no",
    "can_async": true
}
```

This endpoint returns the available file storage drivers (`local`, `dropbox`, `google_drive`, `cloudflare_r2`, `amazon_s3`, plus any driver added by add-ons), the active driver and the background upload option. Remote drivers need Fluent Support Pro (`require_pro`).

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-upload-settings`

## Update File Storage Driver

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/update-remote-upload-driver' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command switches the file upload driver

```json
{
    "message": "Upload driver updated.",
    "driver": "amazon_s3"
}
```

This endpoint switches where new attachments are stored. A remote driver must be connected first (otherwise `Please connect this storage first.`).

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/update-remote-upload-driver`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
driver | string | yes | Driver key: `local`, `dropbox`, `google_drive`, `cloudflare_r2`, `amazon_s3`, or another registered driver

## Update File Storage Options

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-upload-options' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command saves the file storage options

```json
{
    "message": "Storage settings have been saved.",
    "async_upload": "yes"
}
```

This endpoint saves whether files are uploaded to remote storage in the background (Action Scheduler) instead of during the request.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-upload-options`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
async_upload | string | no | `yes` to upload in the background; any other value saves `no`

## Retry Failed Remote Uploads

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-upload-retry' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command queues failed remote uploads again

```json
{
    "message": "3 files will be uploaded again in the background.",
    "queued": 3
}
```

This endpoint queues the files that failed to reach a remote storage again and clears the driver's failure warning. When nothing is queued the message is `The warning was cleared. New uploads will try again.`

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/remote-upload-retry`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
driver | string | yes | A remote driver key (not `local`), e.g. `amazon_s3`

## Get AI Integration Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/ai-integration' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the AI provider settings

```json
{
    "enabled": "yes",
    "provider": "openai",
    "api_key": "****abcd",
    "model": "default",
    "available_models": {
        "openai": [],
        "gemini": [],
        "anthropic": []
    }
}
```

This endpoint returns the AI provider settings. The API key is masked to its last 4 characters. `available_models` lists the model options of each provider.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/ai-integration`

## Save AI Integration Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/ai-integration' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command saves the AI provider settings

```json
{
    "message": "AI settings have been successfully saved."
}
```

This endpoint saves the AI provider, model and API key. An empty or masked (`****...`) key keeps the saved key, but only when the provider is unchanged. When enabled, a key is required (`Please enter your API key.`).

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/ai-integration`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
provider | string | no | `openai` (default), `gemini` or `anthropic`
api_key | string | yes, when enabled | The provider's API key
model | string | no | Model ID; empty uses the provider's default
enabled | string | no | `yes` (default) or `no`

## Get Settings Menu

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/settings-menu' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the settings navigation menu

```json
[
    {
        "title": "Global Settings",
        "route_name": "global_settings",
        "icon": "settings"
    },
    {
        "title": "AI Integration",
        "icon": "aiIntegration",
        "children": [
            {
                "title": "AI Model Setup",
                "route_name": "ai_integration"
            },
            {
                "title": "MCP for AI Agents",
                "route_name": "mcp_settings"
            }
        ]
    }
]
```

This endpoint returns the items of the admin settings menu (used by the admin UI). The list is shortened above.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/settings-menu`

## Get MCP Status

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/mcp' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the MCP (AI agents) status

```json
{
    "mcp_enabled": true,
    "adapter_available": true,
    "toolkit_installed": true,
    "toolkit_active": true,
    "toolkit_version": "1.0.0",
    "can_auto_install": false,
    "toolkit_download_url": "https://github.com/WPManageNinja/fluent-toolkit",
    "endpoint_url": "https://yourdomain.com/wp-json/fluent-support/mcp",
    "tools_count": 30,
    "app_passwords_url": "https://yourdomain.com/wp-admin/profile.php#application-passwords-section",
    "plugins_url": "https://yourdomain.com/wp-admin/plugins.php",
    "current_user_login": "admin",
    "is_local_dev": false
}
```

This endpoint returns everything the "MCP for AI Agents" settings page needs: whether MCP is enabled, whether the FluentHub (fluent-toolkit) adapter is installed/active, the MCP endpoint URL and the number of tools.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/mcp`

## Toggle MCP

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/mcp/toggle' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command enables or disables MCP

```json
{
    "mcp_enabled": true,
    "message": "MCP enabled. AI agents with a valid application password can now reach the Fluent Support tools."
}
```

This endpoint enables or disables the MCP endpoint. Only site administrators (`manage_options`) can change it; others get `success: false` with a permission message.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/mcp/toggle`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
mcp_enabled | boolean or string | yes | `true`, `yes`, `1` or `on` to enable; anything else disables

## Install MCP Adapter

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/mcp/install-adapter' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command installs the FluentHub MCP adapter

```json
{
    "adapter_available": true,
    "toolkit_installed": true,
    "message": "FluentHub installed and activated. The MCP endpoint is ready."
}
```

This endpoint installs and activates the FluentHub (fluent-toolkit) plugin that provides the MCP adapter. It needs the `install_plugins` capability and an active Fluent Pro plugin that supports automatic install; otherwise it returns an error with `toolkit_download_url` for a manual install.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/mcp/install-adapter`

## Get MCP Client Config Snippets

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/mcp/config-snippets' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns MCP client configuration snippets

```json
{
    "snippets": {
        "claude-code": {
            "client": "claude-code",
            "snippet": "claude mcp add \\\n  --transport http \\\n  fluent-support https://yourdomain.com/wp-json/fluent-support/mcp \\\n  --header \"Authorization: Basic <base64(your-username:application-password)>\"",
            "instructions": "Run this in your terminal where Claude Code is installed, replacing the placeholder with base64 of \"username:application-password\"."
        },
        "claude-desktop": {},
        "cursor": {},
        "codex": {},
        "generic": {}
    },
    "endpoint": "https://yourdomain.com/wp-json/fluent-support/mcp",
    "app_passwords_url": "https://yourdomain.com/wp-admin/profile.php#application-passwords-section",
    "is_local_dev": false
}
```

This endpoint returns ready-to-paste MCP configuration for `claude-code`, `claude-desktop`, `cursor`, `codex` and a `generic` client. The snippets contain placeholders for the username and Application Password, never real credentials.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/mcp/config-snippets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
local_dev | string | no | `yes` / `no` to override local-site detection (local mode adds `NODE_TLS_REJECT_UNAUTHORIZED` to the Claude Desktop snippet)

## Get Auto Close Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/auto-close' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the auto close settings

```json
{
    "settings": {
        "enabled": "no",
        "inactive_days": 30,
        "include_tags": [],
        "exclude_tags": [],
        "exclude_if_customer_waiting": "yes",
        "close_silently": "yes",
        "add_close_response": "no",
        "close_response_body": "<p>...</p>",
        "closed_by_agent": "",
        "close_bookmarked_tickets": "no"
    },
    "fields": {}
}
```

This endpoint returns the auto close settings (inactive tickets are closed by an hourly cron) and the form fields.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/auto-close`

## Update Auto Close Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/auto-close' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command saves the auto close settings

```json
{
    "message": "Settings have been updated",
    "settings": {}
}
```

This endpoint saves the auto close settings. Only known keys are saved; missing keys keep their current value. Note that the `settings` key of the response holds the form field definitions. When `enabled` is `yes`, `inactive_days` must be at least 1, a tag cannot be both included and excluded, `closed_by_agent` is required when `exclude_if_customer_waiting` is `no`, and `close_response_body` is required when `add_close_response` is `yes`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/auto-close`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
settings[enabled] | string | no | `yes` / `no`
settings[inactive_days] | int | yes, when enabled | Close tickets with no activity for this many days
settings[include_tags][] | array | no | Only close tickets with these tag IDs
settings[exclude_tags][] | array | no | Never close tickets with these tag IDs
settings[exclude_if_customer_waiting] | string | no | `yes` to skip tickets waiting on an agent reply
settings[close_silently] | string | no | `yes` to close without notifying the customer
settings[add_close_response] | string | no | `yes` to add a closing response
settings[close_response_body] | string (HTML) | yes, when `add_close_response` is `yes` | The closing response
settings[closed_by_agent] | int | yes, when `exclude_if_customer_waiting` is `no` | Fallback agent ID the tickets are closed by
settings[close_bookmarked_tickets] | string | no | `yes` to never auto-close bookmarked (watched) tickets

## Get Incoming Webhook

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/incoming-webhook' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the incoming webhook URL

```json
{
    "webhook": "https://yourdomain.com/wp-json/fluent-support/v2/public/incoming_webhook/WEBHOOK_TOKEN",
    "mailbox": 1,
    "mailboxes": [
        {
            "id": 1,
            "name": "Support",
            "email": "support@example.com"
        }
    ]
}
```

This endpoint returns the incoming webhook URL (which creates tickets from external services) and the mailbox new tickets go to. The webhook is created on first call.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/incoming-webhook`

## Update Incoming Webhook

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/settings/incoming-webhook' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command regenerates the webhook URL or changes its mailbox

```json
{
    "updatedData": 1,
    "message": "Webhook updated successfully"
}
```

This endpoint changes the webhook's mailbox when `mailbox_id` is sent (`Mailbox updated successfully`). Without `mailbox_id` it regenerates the webhook URL with a new token; the old URL stops working.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/settings/incoming-webhook`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
mailbox_id | int | no | Mailbox for tickets created by the webhook. Omit to regenerate the webhook URL

## Get File Upload Integration Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings/upload_integration?integration_key=amazon_s3' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns a storage integration's settings

```json
{
    "settings": {
        "bucket_name": "my-bucket",
        "region": "us-east-1",
        "path_prefix": "",
        "status": "yes",
        "access_key_id": "********",
        "secret_access_key": "********"
    },
    "fieldsConfig": {
        "button_text": "Save Settings",
        "has_discard": true,
        "last_error": "",
        "locked": false,
        "file_count": 12
    }
}
```

This endpoint returns a remote storage integration's settings and form config. Secrets are masked.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings/upload_integration`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
integration_key | string | yes | `amazon_s3`, `dropbox`, `google_drive`, `cloudflare_r2` or `s3_compatible`

## Save File Upload Integration Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings/upload_integration' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command saves and connects a storage integration

```json
{
    "message": "Storage settings have been saved"
}
```

This endpoint saves and verifies a remote storage connection. For OAuth drivers (Dropbox, Google Drive) the response includes a `redirect` URL to authorize the app. Send `settings[disconnect]=1` to disconnect (`is_discarded: true` in the response); files already stored stay reachable. Masked secrets keep the saved value.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings/upload_integration`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
integration_key | string | yes | `amazon_s3`, `dropbox`, `google_drive`, `cloudflare_r2` or `s3_compatible`
settings | object | yes | The driver's settings (see below)

Settings per driver:

* `amazon_s3`: `access_key_id`, `secret_access_key`, `bucket_name`, `region`, `path_prefix`
* `cloudflare_r2`: `access_key_id`, `secret_access_key`, `account_id`, `bucket_name`, `custom_domain`, `path_prefix`
* `s3_compatible`: `access_key_id`, `secret_access_key`, `endpoint`, `region`, `bucket_name`, `path_style`, `custom_domain`, `path_prefix`
* `dropbox` / `google_drive`: `client_id`, `client_secret`
* All drivers: `disconnect` to disconnect, `do_reconnect` to force a reconnect
