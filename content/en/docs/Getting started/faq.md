---
title: "FAQ"
linkTitle: "FAQ"
weight: 6
description: >
  Frequently Asked Questions
---

#### Can I change the time between Command/WebHook resend availability

- **TLDR:** Change the Interval of the Service (See [options.interval](../Config/service/#options)). The minimum time between successful action repeats is `2*Interval`.

When a Command/WebHook is triggered, no resends are attempted for at least an hour, or until
  a) the Command has finished,
  b) the WebHook was received, or
  c) the WebHook wasn't received after `max_tries` sends.

This has been done to prevent multiple attempts of the same version upgrade from being tried at once.

If the Command/WebHook failed, the time until a resend is possible is `15s`.

If the Command/WebHook succeeded, the time until a resend is possible is `2*Interval` (of the parent Service).

This time is not configurable beyond setting the interval of service and has been chosen to help ensure the upgrade has had time to complete and will be noticed by a `deployed_version` check (if it has one) before a resend is possible.


#### What config vars support environment variables?

Environment variables in the format `${ENV_VAR}` (e.g. 'abc ${AUTH_TOKEN}'  or just '${ENV_VAR}') can be used in the following fields:

* settings.web.basic_auth.password
* settings.web.basic_auth.username
* deployed_version.basic_auth.password
* deployed_version.basic_auth.username
* deployed_version.headers.\*.key
* deployed_version.headers.\*.value
* deployed_version.url
* latest_version.access_token
* latest_version.require.docker.token
* latest_version.require.docker.username
* latest_version.url
* notify.\*.options.\*
* notify.\*.params.\*
* notify.\*.url_fields.\*
* webhook.\*.custom_headers.\*.key
* webhook.\*.custom_headers.\*.value
* webhook.\*.secret
* webhook.\*.url
