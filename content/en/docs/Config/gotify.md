---
title: "Gotify"
linkTitle: "Gotify"
weight: 3
description: >
  Configure global Gotify notifiers that can be used by any Service.
---

Create an application on the Gotify web UI, and use that URL with the token for the application you make.

config.yml:
```
gotify:
  ...
  # As many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_GOTIFY_ID:
    url: https://GOTIFY_URL  # Gotify URL to send to
    token: TOKEN             # Token to use
    title: TITLE             # Title to use in the messages
    message: MESSAGE         # Message template
    priority: 5              # Priority of the message - <1 = Min, 1-3 = Low, 4-7 = Med, >7 = High
    extras:
      android_action: '{{ web_url }}'       # URL to open on notification delivery
      cli3nt_display: 'text/plain'          # How the notification will be rendered, either 'text/plain' (default),
                                            # or 'text/markdown'
      client_notification: '{{ web_url }}'  # URL to open on notification click
    delay: 0s                # Time to wait when a new release is found before sending this message
    max_tries: 3             # Maximum number of times to try sending this message until a send is successful
```

#### Extras
For more info on the above-mentioned extras, look for them [here](https://gotify.net/docs/msgextras).


#### title/message/extras templating

The `title`, `message` and `extras` vars used in the Gotify messages can be customised with Django-style templating courtesy of [pongo2](https://www.schlachter.tech/solutions/pongo2-template-engine/).

The default message template is `{{ service_id }} - {{ version }} released`, which with a `service_id` of 'example_service' and the version that triggered
this message being '1.2.3', woud trigger a message of
`example_service - 1.2.3 released`.

The vars that can be used in the templates are:
- service_id
- service_url
- web_url
- version

So, for example, if you had `web_url` as a link to the changelog for new releases, you could use a message template of
`{{ service_id }} - {{ version }} released. {{ web_url }}` to include a link to the changelog in the message.

For further guidance and other helpful examples on the templating used, start by looking [here](/docs/help/templating).
