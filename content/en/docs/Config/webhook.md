---
title: "Webhook"
linkTitle: "Webhook"
weight: 3
description: >
  Configure global Webhook(s) that can be used by any Service.
---

## Using GitHub-style Webhooks
With AWX for example, to get a Webhook URL and a Webhook Key, edit a template and go to
'Options' at the bottom. There, tick the 'Enable Webhook' box. In the 'Webhook details'
that appears, set the 'Webhook Service' to 'GitHub' and save the changes to get both
the URL and Key for this Webhook.

{{< alert title="Note" >}}
Environment variables in the format ${ENV_VAR} can be used in the `headers.*.key`, `headers.*.value`, `secret` and `url` fields.
{{< /alert >}}

config.yml:
```yaml
webhook:
  ...
  # As many of these (below) as you like, just ensure they have unique IDs
  EXAMPLE_WEBHOOK_ID:
    type: github                # Type of Webhook to send
    url: https://WEBHOOK_URL    # Webhook URL to send to
    allow_invalid_certs: false  # Accept invalid HTTPS certs/not
    secret: SECRET              # Webhook Key/Secret
    headers:                    # Custom headers to give the Webhook (could be a param for the Webhook that's unique to the Service)
      - key: fizz
        value: bang
    desired_status_code: 201    # Status code to use indicating a success. Using 0 will accept any 2XX status code
    delay: 0s                   # Delay before sending the webhooks when a new release is found
                                # (only used when `auto_approve` is true for the service)
    max_tries: 3                # Maximum number of times to try sending this message until a send is successful
    silent_fails: false         # Whether to notify the service's notifiers if max_tries fails occur
```

### headers
Say you had one script that could handle a few different services, `headers` could be used to give the receiver of the Webhook a param that tells it which service to update.
If using something like [adnanh/webhook](https://github.com/adnanh/webhook), you can setup params for your Webhook with something like:

```yaml
- id: something
  execute-command: something.sh
  pass-arguments-to-command:
    - source: header
      name: SOMETHING
    - source: header
      name: VERSION
```

Then, in Argus' `config.yml`
```yaml
    webhook:
      some_name:
        headers:
          - key: SOMETHING
            value: 'foo'
          - key: VERSION
            value: '{{ version }}'
```

would execute ["something.sh" "foo" "1.2.3"] (assuming the latest_version is '1.2.3')

## Using GitLab Webhooks
GitLab webhooks can be used to start GitLab CI pipelines through which the applications are updated.
For this, a `Pipeline trigger` must be created in GitLab in the project settings under `CI/CD`.

[Official Docs](https://docs.gitlab.com/ee/ci/triggers/#use-a-webhook)

The `token` must be stored as secret and doesn't have to be added to the URL.

Example URL: `https://gitlab.com/api/v4/projects/<project_id>/ref/<ref_name>/trigger/pipeline`

config.yml
```yaml
webhook:
  ...
  # As many of these (below) as you like, just ensure they have unique IDs
  EXAMPLE_WEBHOOK_ID:
    type: gitlab                # Type of Webhook to send
    url: https://WEBHOOK_URL    # Webhook URL to send to
    allow_invalid_certs: false  # Accept invalid HTTPS certs/not
    secret: SECRET              # Webhook Token
    desired_status_code: 201    # Status code to use indicating a success. Using 0 will accept any 2XX status code
    delay: 0s                   # Delay before sending the webhooks when a new release is found
                                # (only used when `auto_approve` is true for the service)
    max_tries: 3                # Maximum number of times to try sending this message until a send is successful
    silent_fails: false         # Whether to notify the service's notifiers if max_tries fails occur
```

### Custom variables
To use a GitLab CI for multiple services or functions, you can specify custom variables (like when triggering a pipeline via the Web UI).

Example:
To set var `PATCH` to `true` and set `SERVICE` to `argus` you must append the following to the URL.

`?variables[PATCH]=true&variables[SERVICE]=argus`

Then, in Argus' `config.yml`
```yaml
    webhook:
      some_name:
        url: https://gitlab.com/api/v4/projects/<Project ID>/ref/<Branch/Tag>/trigger/pipeline?variables[PATCH]=true&variables[SERVICE]=argus
```