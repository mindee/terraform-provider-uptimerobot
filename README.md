# Terraform UptimeRobot Provider

[![Build Status](https://github.com/mindee/terraform-provider-uptimerobot/workflows/test/badge.svg)](https://github.com/mindee/terraform-provider-uptimerobot/actions?query=branch%3Amaster+workflow%3Atest)

## Getting started

To install this provider, check out the installation instructions on [Terraform's registry page](https://registry.terraform.io/providers/mindee/uptimerobot/latest).

```tf
terraform {
  required_providers {
    uptimerobot = {
      source = "mindee/uptimerobot"
      version = "1.0.0"
    }
  }
}

provider "uptimerobot" {
  api_key = "[YOUR MAIN API KEY]" # or pass via environment variable UPTIMEROBOT_API_KEY
}

data "uptimerobot_account" "account" {}

data "uptimerobot_alert_contact" "default_alert_contact" {
  friendly_name = data.uptimerobot_account.account.email
}

resource "uptimerobot_alert_contact" "slack" {
  friendly_name = "Slack Alert"
  type          = "slack"
  value         = "https://hooks.slack.com/services/XXXXXXX"
}

resource "uptimerobot_monitor" "main" {
  friendly_name = "My Monitor"
  type          = "http"
  url           = "http://example.com"
  # pro allows 60 seconds
  interval      = 300

  alert_contact {
    id = uptimerobot_alert_contact.slack.id
    # threshold  = 0  # pro only
    # recurrence = 0  # pro only
  }

  alert_contact {
    id = data.uptimerobot_alert_contact.default_alert_contact.id
  }
}

resource "uptimerobot_monitor" "custom_port" {
  url           = "doe.john.me"
  type          = "port"
  sub_type      = "custom"
  port          = 5678
  friendly_name = "Custom port"
}

resource "uptimerobot_status_page" "main" {
  friendly_name  = "My Status Page"
  custom_domain  = "status.example.com"
  password       = "WeAreAwsome"
  sort           = "down-up-paused"
  monitors       = [uptimerobot_monitor.main.id]
}

resource "aws_route53_record" {
  zone_id = "[MY ZONE ID]"
  type    = "CNAME"
  records = [uptimerobot_status_page.main.dns_address]
}

```
