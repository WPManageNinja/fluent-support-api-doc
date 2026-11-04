---
title: Fluent Support REST API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - php

toc_footers:
  - <a href='https://fluentsupport.com/docs'>User Documentation</a>
  - <a href='https://wordpress.org/plugins/fluent-support/'>Download Fluent Support</a>
  - <a href='https://fluentsupport.com'>Fluent Support Official Website</a>

includes:
  - tickets
  - tickets_pro
  - customers
  - customer_portal
  - auth
  - agents
  - agent_groups
  - mailboxes
  - products
  - ticket_tags
  - ticket_custom_fields
  - saved_replies
  - workflows
  - time_tracking
  - ai
  - reports
  - personal_reports
  - advanced_reports
  - activities
  - notifications
  - settings
  - ticket_form
  - license
  - remote_portal
  - system_tools
  - ticket_importer
  - webhooks
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the Fluent Support API document. This document will describe the REST API Endpoints of Fluent Support.

The example API documentation page was created with [Slate](https://github.com/slatedocs/slate). If you find any typo or would like contribute please send a pull request with improvements. Endpoints marked with a notice require Fluent Support Pro.

# Authentication

Fluent Support uses WordPress REST API. So you can use any authorization method that supports WordPress. The easiest way to connect is application password. To create a application password go to the **Users** and click on the user and create a new application password for this user so that this person can easily access the REST API endpoint by using WP Username & Application password in basic auth system.

Once you create your Application Password in WordPress, Add Authorization Header to every request.

In the basic auth username section use your WP Username and in the password section use the created Application Password.
> Example API Call for tickets

```shell
# With shell, you can just pass the correct header with each request
curl "https://yourdomain.com/wp-json/fluent-support/v2/" \
  -H "Authorization: BASIC API_USERNAME:API_PASSWORD"
```

**API Base URL:** `https://yourdomain.com/wp-json/fluent-support/v2`

> Make sure to replace `API_USERNAME` & `API_PASSWORD` with your UserName & API Password.
