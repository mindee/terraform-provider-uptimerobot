---
layout: "uptimerobot"
page_title: "UptimeRobot: uptimerobot_monitor"
sidebar_current: "docs-uptimerobot-resource-monitor"
description: |-
  Set up a monitor
---

# Resource: uptimerobot_monitor

Use this resource to create a monitor in UptimeRobot.

## Example Usage

```hcl
resource "uptimerobot_monitor" "my_website" {
  friendly_name = "My Monitor"
  type          = "http"
  url           = "http://example.com"
}
```

## Arguments Reference

* `friendly_name` - friendly name of the monitor (for making it easier to distinguish from others).
* `url` - the URL/IP of the monitor.
* `type` - the type of the monitor. Can be one of the following:
  - *`http`*
  - *`keyword`* - will also enable the following options:
    - `keyword_type` - if the monitor will be flagged as down when the keyword exists or not exists. Can be one of the following:
      - `exists`
      - `not exists`
    - `keyword_value` - the value of the keyword.
  - *`ping`*
  - *`port`* - will also enable the following options:
    - `sub_type` - which pre-defined port/service is monitored or if a custom port is monitored. Can be one of the following:
      - `http`
      - `https`
      - `ftp`
      - `smtp`
      - `pop3`
      - `imap`
      - `custom`
    - `port` - the port monitored (only if subtype is `custom`)
* `http_username` - used for password-protected web pages (HTTP basic or digest). Available for HTTP and keyword monitoring.
* `http_password` - used for password-protected web pages (HTTP basic or digest). Available for HTTP and keyword monitoring.
* `http_auth_type` - Used for password-protected web pages (HTTP basic or digest). Available for HTTP and keyword monitoring. Can be one of the following:
  - `basic`
  - `digest`
* `interval` - the interval for the monitoring check (300 seconds by default).
* `timeout` - timeout duration for monitoring check (30 seconds by default).
* `http_method` - the HTTP method to be used. Can be one of the following:
  - `GET`
  - `POST`
  - `PUT`
  - `PATCH`
  - `DELETE`
  - `OPTIONS`
* `post_content_type` - sets the Content-Type for POST, PUT, PATCH, DELETE, OPTIONS HTTP methods. Can be one of the following:
  - `text/html`
  - `application/json`
* `post_type` - the format of data to be sent with POST, PUT, PATCH, DELETE, OPTIONS HTTP methods. Can be one of the following:
  - `key-value`
  - `raw`
* `post_value` - the data to be sent with POST, PUT, PATCH, DELETE, OPTIONS HTTP methods. Must be a JSON object.
* `alert_contact` - the alert contact to notify. The options are:
  - `id` - the ID of the alert contact to notify
  - `threshold` - the maximum number of recurrence of the alert to send
  - `recurrence` - the number of recurrence of the alert to send
* `ignore_ssl_errors` - for ignoring SSL certificate related errors. `false` by default.
* `custom_http_headers` - insert custom HTTP headers into the monitor. Must be a JSON object.

## Attributes Reference

* `id` - the ID of the monitor (can be used for monitor-specific requests)
* `status` - the status of the monitor (`paused`, `not checked yet`, `up`, `seems down`, or `down`)

## Import

Import is supported using the following syntax:

```shell
# Monitor can be imported by specifying the numeric identifier.
terraform import uptimerobot_monitor.example 123
```
