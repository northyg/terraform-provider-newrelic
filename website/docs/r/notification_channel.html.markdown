---
layout: "newrelic"
page_title: "New Relic: newrelic_notification_channel"
sidebar_current: "docs-newrelic-resource-notification-channel"
description: |-
Create and manage a notification channel for notifications in New Relic.
---

# Resource: newrelic\_notification\_channel

Use this resource to create and manage New Relic notification channels. Details regarding supported products and permissions can be found [here](https://docs.newrelic.com/docs/alerts-applied-intelligence/notifications/destinations).

## Example Usage

##### [Webhook](https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-api-notifications-channels/#webhook)
```hcl
resource "newrelic_notification_channel" "foo" {
  name = "webhook-example"
  type = "WEBHOOK"
  destination_id = "00b6bd1d-ac06-4d3d-bd72-49551e70f7a8"
  product = "IINT"

  properties {
    key = "payload"
    value = "{\n\t\"name\": \"foo\"\n}"
    label = "Payload Template"
  }
}
```
See additional [examples](#additional-examples).

## Argument Reference

The following arguments are supported:

* `account_id` - (Optional) Determines the New Relic account where the notification channel will be created. Defaults to the account associated with the API key used.
* `name` - (Required) The name of the channel.
* `type` - (Required) The type of channel.  One of: `EMAIL`, `SERVICENOW_INCIDENTS`, `WEBHOOK`, `JIRA_CLASSIC`, `JIRA_NEXTGEN`, `PAGERDUTY_ACCOUNT_INTEGRATION` or `PAGERDUTY_SERVICE_INTEGRATION`.
* `destination_id` - (Required) The id of the destination.
* `product` - (Required) The type of product.  One of: `DISCUSSIONS`, `ERROR_TRACKING` or `IINT` (workflows).
* `properties` - A nested block that describes a notification channel properties. See [Nested properties blocks](#nested-properties-blocks) below for details.

### Nested `properties` blocks

Each notification channel type supports a specific set of arguments for the `properties` block:

* `WEBHOOK`
  * `headers` - (Optional) A map of key/value pairs that represents the webhook headers.
  * `payload` - (Required) A map of key/value pairs that represents the webhook payload.
* `SERVICENOW_INCIDENTS`
  * `description` - (Optional) A map of key/value pairs that represents a description.
  * `short_description` - (Optional) A map of key/value pairs that represents a short description.
* `JIRA_CLASSIC`, `JIRA_NEXTGEN`
  * `project` - (Required) A map of key/value pairs that represents the jira project id.
  * `issuetype` - (Required) A map of key/value pairs that represents the issue type id.
* `EMAIL`
  * `subject` - (Optional) A map of key/value pairs that represents the email subject title.
  * `customDetailsEmail` - (Optional) A map of key/value pairs that represents the email custom details.
* `PAGERDUTY_SERVICE_INTEGRATION`
  * `summary` - (Required) A map of key/value pairs that represents the summery.
* `PAGERDUTY_ACCOUNT_INTEGRATION`
  * `summary` - (Required) A map of key/value pairs that represents the summery.
  * `service` - (Required) Specifies the service id for integrating with Pagerduty.
  * `email` - (Required) Specifies the user email for integrating with Pagerduty.

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The ID of the channel.

## Additional Examples

##### [ServiceNow](https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-api-notifications-channels/#servicenow)
```hcl
resource "newrelic_notification_channel" "foo" {
  name = "servicenow-incident-example"
  type = "SERVICENOW_INCIDENTS"
  destination_id = "00b6bd1d-ac06-4d3d-bd72-49551e70f7a8"
  product = "IINT"

  properties {
    key = "description"
    value = "General description"
  }

  properties {
    key = "short_description"
    value = "Short description"
  }
}
```

##### [Email](https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-api-notifications-channels/#email)
```hcl
resource "newrelic_notification_channel" "foo" {
  name = "email-example"
  type = "EMAIL"
  destination_id = "00b6bd1d-ac06-4d3d-bd72-49551e70f7a8"
  product = "ERROR_TRACKING"

  properties {
    key = "subject"
    value = "New Subject Title"
  }
}
```

##### [Jira Classic](https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-api-notifications-channels/#jira)
```hcl
resource "newrelic_notification_channel" "foo" {
  name = "jira-example"
  type = "JIRA_CLASSIC"
  destination_id = "00b6bd1d-ac06-4d3d-bd72-49551e70f7a8"
  product = "ERROR_TRACKING"
  
  properties {
    key = "project"
    value = "10000"
  }

  properties {
    key = "issuetype"
    value = "10004"
  }

  properties {
    key = "description"
    value = "Issue ID: {{ issueId }}"
  }

  properties {
    key = "summary"
    value = "{{ annotations.title.[0] }}"
  }
}
```

##### [PagerDuty with account integration](https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-api-notifications-channels/#pagerduty)
```hcl
resource "newrelic_notification_channel" "foo" {
  name = "pagerduty-account-example"
  type = "PAGERDUTY_ACCOUNT_INTEGRATION"
  destination_id = "00b6bd1d-ac06-4d3d-bd72-49551e70f7a8"
  product = "IINT"

  properties {
    key = "summary"
    value = "General summary"
  }

  properties {
    key = "service"
    value = "PTQK3FM"
  }

  properties {
    key = "email"
    value = "example@email.com"
  }
}
```

##### [PagerDuty with service integration](https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-api-notifications-channels/#pagerduty)
```hcl
resource "newrelic_notification_channel" "foo" {
  name = "pagerduty-account-example"
  type = "PAGERDUTY_SERVICE_INTEGRATION"
  destination_id = "00b6bd1d-ac06-4d3d-bd72-49551e70f7a8"
  product = "IINT"

  properties {
    key = "summary"
    value = "General summary"
  }
}
```

~> **NOTE:** Sensitive data such as channel API keys, service keys, etc are not returned from the underlying API for security reasons and may not be set in state when importing.

## Additional Information
More details about the channels API can be found [here](https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-api-notifications-channels).