---
title: "Defaults"
linkTitle: "Defaults"
weight: 2
description: >
  Defaults to give each service/gotify/slack/webhook.
---

You can define defaults for [service](/docs/config/service), [gotify](/docs/config/gotify), [slack](/docs/config/slack) and [webhook](/docs/config/webhook) in `config.yml` like so:


#### **service** portion
```yaml
defaults:
  ...
  service:
    auto_approve: false           # Whether approval is required on the web UI for sending the new release WebHooks
    allow_invalid_certs: false    # Whether invalid HTTPS certs are allowed in queries
    ignore_misses: true           # Whether url_command misses will be reported in the logs
    interval: 10m                 # How often to query for new releases
    use_prerelease: false         # Whether 'prerelease' GitHub tags can be used
    semantic_versioning: true     # Whether to enforce semantic versioning (required to only alert on newew versions)
    deployed_version:             # Get the `current_version` from a deployed service
      allow_invalid_certs: false  # Accept invalid HTTPS certs/not
```

#### **gotify** portion
```yaml
defaults:
  ...
  gotify:
    delay: 0s                                             # Delay before release notifications
    max_tries: 3                                          # Maximum number of tries at sending each message
    title: Argus                                          # Title template
    message: '{{ service_id }} - {{ version }} released'  # Message template
    priority: 5                                           # Priority to give the messages
```

#### **slack** portion
```yaml
defaults:
  ...
  slack:
    delay: 0s                                 # Delay before release notifications
    max_tries: 3                              # Maximum number of tries at sending each message
    username: Argus                           # Username template
    icon_emoji: ':github:'                    # Sender icon to use in the message
    message: >-                               # Message template
      <{{ service_url }}|{{ service_id }}> -
      {{ version }} released{% if web_url %}
      (<{{ web_url }}|changelog>){% endif %}
```

#### **webhook** portion
```yaml
defaults:
  ...
  webhook:
    delay: 0s                 # Delay before sending the webhooks when a new release is found
                              # (only used when auto_approve is true for the service)
    max_tries: 3              # Maximum number of tries at sending each webhook
    desired_status_code: 201  # Status code to use indicating a success. Using 0 will accept any 2XX status code
    silent_fails: false       # Whether to notify the service's notifiers if max_tries fails occur
```
