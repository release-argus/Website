---
title: "WebHook"
linkTitle: "WebHook"
weight: 3
description: >
  Configure global WebHook(s) that can be used by any Service.
---

With AWX for example, to get a WebHook URL and a WebHook Key, edit a template and go to
'Options' at the bottom. There, tick the 'Enable Webhook' box. In the 'Webhook details'
that appears, set the 'Webhook Service' to 'GitHub' and save the changes to get both
the URL and Key for this WebHook.

config.yml:
```yaml
webhook:
  ...
  # As many of these (below) as you like, just ensure they have unique ID's
  EXAMPLE_WEBHOOK_ID:
    type: github                # Type of WebHook to send
    url: https://WEBHOOK_URL    # WebHook URL to send to
    allow_invalid_certs: false  # Accept invalid HTTPS certs/not
    secret: SECRET              # WebHook Key/Secret
    custom_headers:             # Custom headers to give the WebHook (could be a param for the WebHook that's unique
      fizz: bang                # to the Service)
    desired_status_code: 201    # Status code to use indicating a success. Using 0 will accept any 2XX status code
    delay: 0s                   # Delay before sending the webhooks when a new release is found
                                # (only used when `auto_approve` is true for the service)
    max_tries: 3                # Maximum number of times to try sending this message until a send is successful
    silent_fails: false         # Whether to notify the service's notifiers if max_tries fails occur
```

## custom_headers
Say you had one script that could handle a few different services, `custom_headers` could be used to give the receiver of the WebHook a param that tells it which service to update.
If using something like [adnanh/webhook](https://github.com/adnanh/webhook), you can setup params for your WebHook with something like:

```yaml
- id: something
  execute-command: SOMETHING.SH
  pass-arguments-to-command:
  - source: header
    name: CUSTOM_HEADER_NAME
```

Then, in Argus' `config.yml`
```yaml
    webhook:
      some_name:
        custom_headers:
          CUSTOM_HEADER_NAME: hello123
```

would execute ["SOMETHING.SH" "hello123"]