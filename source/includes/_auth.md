# Customer Authentication

These public endpoints power the customer portal's own login, signup, two-factor login and password reset forms. They need no API credentials, but each one (except two-factor verification) checks a WordPress nonce that is printed in the portal's forms, so they are meant to be called from the portal page in the browser. Login, signup and password reset only work while Fluent Support's own forms are in use (not when FluentAuth handles authentication). All of them are rate limited and return a 429 error when the limit is hit.

## Customer Signup
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/signup' \
--header 'Content-Type: application/json' \
--data-raw '{
    "_fsupport_signup_nonce": "a1b2c3d4e5",
    "first_name": "Jane",
    "last_name": "Doe",
    "username": "janedoe",
    "email": "jane@example.com",
    "password": "a-strong-password"
}'
```

> The first request emails a verification code and returns the code form

```json
{
    "verification_html": "<div class=\"fs_signup_verification\">...</div>"
}
```

> Sending the same data again with the code creates and logs in the user

```json
{
    "message": "Successfully registered to the site.",
    "redirect": "https://yourdomain.com/support-portal/"
}
```

This endpoint registers a new WordPress user as a customer in two steps. The first request validates the form and emails a verification code. The second request repeats the form data with `_email_verification_hash` and `_email_verification_token`; the account is created for the email the code was sent to, and the user is logged in. User registration must be enabled. If reCAPTCHA is enabled for the signup form, the first request needs a valid `g-recaptcha-response`.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/signup`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
_fsupport_signup_nonce | text | yes | Nonce from the portal signup form (`fluent_support_signup_nonce`)
first_name | text | yes | First name
last_name | text | no | Last name
username | text | yes | Username
email | email | yes | Email address
password | text | yes | Password, at least 8 characters
g-recaptcha-response | text | no | reCAPTCHA token, when reCAPTCHA is enabled (first step only)
_email_verification_hash | text | no | Verification hash from the code form (second step)
_email_verification_token | text | no | The code emailed to the user (second step)
__redirect_to | url | no | Where to redirect after signup; must be on the same site

## Customer Login
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "_support_login_nonce": "a1b2c3d4e5",
    "log": "jane@example.com",
    "pwd": "a-strong-password",
    "rememberme": true
}'
```

> The above command will log in a user

```json
{
    "redirect": "https://yourdomain.com/support-portal/"
}
```

> When email two-factor login is enabled

```json
{
    "load_2fa": "yes",
    "two_fa_form": "<form class=\"fs_2fa_form\">...</form>"
}
```

This endpoint logs a user in with their username or email and password. When two-factor login is enabled in the settings, a login code is emailed instead and the code form is returned; finish the login with the two-factor endpoint. Wrong credentials and unknown accounts get the same error. If reCAPTCHA is enabled for the login form, a valid `g-recaptcha-response` is required.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/login`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
_support_login_nonce | text | yes | Nonce from the portal login form (`fsupport_login_nonce`)
log | text | yes | Username or email
pwd | text | yes | Password
rememberme | boolean | no | Keep the user logged in
redirect_to | url | no | Where to redirect after login; must be on the same site
g-recaptcha-response | text | no | reCAPTCHA token, when reCAPTCHA is enabled

## Verify Two-Factor Login Code
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/two_fa' \
--header 'Content-Type: application/json' \
--data-raw '{
    "login_hash": "f3a9b7c6d5e4",
    "login_passcode": "123456"
}'
```

> The above command will verify the login code and log in the user

```json
{
    "redirect": "https://yourdomain.com/support-portal/"
}
```

This endpoint is the second step of the email two-factor login. It checks the emailed code and logs in the user it was issued to.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/two_fa`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
login_hash | text | yes | The hash from the two-factor form returned by the login endpoint
login_passcode | text | yes | The code emailed to the user

## Reset Password
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/reset_pass' \
--header 'Content-Type: application/json' \
--data-raw '{
    "_fsupport_reset_pass_nonce": "a1b2c3d4e5",
    "user_login": "jane@example.com"
}'
```

> The above command will send a password reset link

```json
{
    "message": "If an account matches that username or email, a password reset link has been sent. Please check your inbox, including the spam folder."
}
```

This endpoint emails a password reset link to the matching user. It always returns the same message, whether or not an account exists. A new link is sent at most once every 5 minutes per account.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/reset_pass`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
_fsupport_reset_pass_nonce | text | yes | Nonce from the portal reset password form (`fluent_support_reset_pass_nonce`)
user_login | text | yes | Username or email
