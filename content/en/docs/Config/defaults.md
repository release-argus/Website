---
title: "Defaults"
linkTitle: "Defaults"
weight: 2
description: >
  Defaults to give each service/notify/slack/webhook.
---

You can define defaults for [service](/docs/config/service), [notify](/docs/config/notify), [slack](/docs/config/slack) and [webhook](/docs/config/webhook) in `config.yml` like so:


#### **service** portion
```yaml
defaults:
  ...
  service:
    access_token: ''              # GitHub access token to increase your rate-limit and/or access private repos
                                  # https://github.com/settings/tokens - w/ repo.public_repo/repo for public/private
    auto_approve: false           # Whether approval is required on the web UI for sending the new release WebHooks
    allow_invalid_certs: false    # Whether invalid HTTPS certs are allowed in queries
    ignore_misses: true           # Whether url_command misses will be reported in the logs
    interval: 10m                 # How often to query for new releases
    use_prerelease: false         # Whether 'prerelease' GitHub tags can be used
    semantic_versioning: true     # Whether to enforce semantic versioning (required to only alert on newew versions)
    deployed_version:             # Get the `current_version` from a deployed service
      allow_invalid_certs: false  # Accept invalid HTTPS certs/not
```

#### **notify** portion
Defaults can be defined for each `notify` type. Details about the vars for each type can be found [here](/docs/config/notify).

```yaml
defaults:
  ...
  notify:
    discord:
    email:
    gotify:
    googlechat:
    ifttt:
    join:
    mattermost:
    matrix:
    opsgenie:
    pushbullet:
    pushover:
    rocketchat:
    slack:
    teams:
    telegram:
    zulip:
    shoutrrr:
```


#### **webhook** portion
```yaml
defaults:
  ...
  webhook:
    delay: 0s                   # Delay before sending the webhooks when a new release is found
                                # (only used when auto_approve is true for the service)
    max_tries: 3                # Maximum number of tries at sending each webhook
    allow_invalid_certs: false  # Whether invalid HTTPS certs are allowed in this webhook
    desired_status_code: 201    # Status code to use indicating a success. Using 0 will accept any 2XX status code
    silent_fails: false         # Whether to notify the service's notifiers if max_tries fails occur
```
