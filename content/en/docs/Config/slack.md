---
title: "Slack"
linkTitle: "Slack"
weight: 3
description: >
  Configure global Slack notifiers that can be used by any Service.
---

Create an incoming webhook by following [this guidance](https://api.slack.com/messaging/webhooks).

config.yml:
```
slack:
  ...
  # As many of these (below) as you like, just ensure they have unique ID's
  EXAMPLE_SLACK_ID:
    url: https://SLACK_WEBHOOK_URL           # Slack URL to send to
    icon_emoji: ':github:'                   # Emoji to use as the sender icon
    icon_url: https://example.com/image.png  # Link to an icon to use as the sender icon
    username: Hymenaios                      # Username of the sender
    message: MESSAGE                         # Message template
    delay: 0s                                # Time to wait when a new release is found before sending this message
    max_tries: 3                             # Maximum number of times to try sending this message until a send is
                                             # successful
```
{{< alert title="Note" >}}
don't set both an `icon_emoji` and a `icon_url` as Hymenaios will always use `icon_url` over `icon_emoji`.
{{< /alert >}}

#### message templating

The `message` used in the Slack messages can be customised with Django-style templating courtesy of [pongo2](https://www.schlachter.tech/solutions/pongo2-template-engine/).

The default message template is
`<{{ service_url }}|{{ service_id }}> - {{ version }} released{% if web_url %} (<{{ web_url }}|changelog>){% endif %}`
, which with a `service_id` of 'example_service', a `service_url` of 'example.com', no `web_url` and the `version` that triggered
this message being '1.2.3', woud trigger a message of `example_service - 1.2.3 released` (example_service would be a clickable link to the service_url). If the Service had a `web_url` defined, then ' (changelog)' would appear at the end, where the 'changelog' text would be a clickable link to that `web_url`.

The vars that can be used in both the title and message templates are:
- service_id
- service_url
- web_url
- version

The default Slack message format assumes that when `web_url` is defined, it is a link to the services changelog. Then, in any notifications by Hymenaios about that service,
you should get a clickable 'changelog' button that takes you to that `web_url`.


For further guidance and other helpful examples on the templating used, start by looking [here](/docs/help/templating).
