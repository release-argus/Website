---
title: "FAQ"
linkTitle: "FAQ"
weight: 6
description: >
  Frequently Asked Questions
---

#### Can I change the time between Command/WebHook resends
When a Command/WebHook is triggered, resends are blocked for an hour, or until the Command has finished/the WebHook was receieved/the WebHook wasn't receieved after `max_tries` sends. This has been done to prevent multiple of the same version upgrade from being attempted at once. If the Command/WebHook failed, the time until a resned is possible is `15s`. If the Command/WebHook succeeded, the time until a resend is possible is `2*Interval` (of the parent Service). This time is not configurable and has been chosen to help ensure the upgrade has had time to complete and will be noticed by a `deployed_version` check (if it has one) before a resend is possible.
- **TLDR:** Change the Interval of the Service. Minimum time between successful action repeats is `2*Interval`