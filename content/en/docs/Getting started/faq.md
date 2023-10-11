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
  b) the WebHook was receieved, or
  c) the WebHook wasn't receieved after `max_tries` sends.

This has been done to prevent multiple attempts of the same version upgrade from being tried at once.

If the Command/WebHook failed, the time until a resend is possible is `15s`.

If the Command/WebHook succeeded, the time until a resend is possible is `2*Interval` (of the parent Service).

This time is not configurable beyond setting the interval of service and has been chosen to help ensure the upgrade has had time to complete and will be noticed by a `deployed_version` check (if it has one) before a resend is possible.

