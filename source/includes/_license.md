# License

The license endpoints manage the Fluent Support Pro license on this site. They require the `fst_manage_settings` permission. The raw license key and activation hash are never returned.

## Get License Status

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/pro/license' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the license status

```json
{
    "status": "valid",
    "variation_id": "2",
    "variation_title": "Agency License",
    "expires": "2027-01-15 00:00:00",
    "last_checked": "2026-11-04 10:12:30",
    "renew_url": "",
    "is_expired": false,
    "purchase_url": "https://fluentsupport.com/",
    "account_url": "https://wpmanageninja.com/account/",
    "support_url": "https://wpmanageninja.com/account/support-tickets/",
    "license_key_masked": "ABCD****WXYZ",
    "product_title": "FluentSupport Pro",
    "is_lifetime": false,
    "expires_human": "January 15, 2027",
    "days_remaining": 72,
    "last_checked_human": "1 min ago",
    "in_grace_period": false,
    "grace_period_days": null
}
```

This endpoint checks the license with the store and returns its status. `status` is `valid`, `expired`, `unregistered`, `invalid` or `error`. When the store cannot be reached it returns `status: invalid`, `error: true`, `message`, `purchase_url` and `support_url`. A `renew_url` is included only when a renewal is offered (expired, in grace period, or expiring within 30 days).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/pro/license`

## Activate License

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/pro/license' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command activates a license key

```json
{
    "license_data": {
        "status": "valid",
        "expires": "2027-01-15 00:00:00",
        "license_key_masked": "ABCD****WXYZ",
        "product_title": "FluentSupport Pro",
        "is_lifetime": false
    },
    "message": "Your license key has been successfully updated"
}
```

This endpoint activates a license key on this site. `license_data` has the same shape as [Get License Status](#get-license-status). If the store refuses the key, an error with its `message` is returned.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/pro/license`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
license_key | string | yes | The Fluent Support Pro license key

## Deactivate License

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/pro/remove-license' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command deactivates the license

```json
{
    "license_data": {
        "status": "unregistered",
        "variation_id": "",
        "variation_title": "",
        "expires": "",
        "license_key_masked": "",
        "product_title": "FluentSupport Pro",
        "is_lifetime": false
    },
    "message": "Your license key has been successfully deactivated"
}
```

This endpoint deactivates the license on the store and removes it from this site.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/pro/remove-license`
