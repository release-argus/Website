---
title: "Testing"
linkTitle: "Testing"
weight: 5
description: >
  Testing your config
---

## Test a Gotify connection
```
❯ ./hymenaios -test.gotify EXAMPLE_GOTIFY
Loading config from "config.yml"
INFO: Testing (EXAMPLE_GOTIFY)
INFO: Message sent succesfully with "EXAMPLE_GOTIFY" config
```

## Test a Slack connection
```
❯ ./hymenaios -test.slack EXAMPLE_SLACK
INFO: Testing (EXAMPLE_SLACK)
INFO: Message sent succesfully with "EXAMPLE_SLACK" config
```

## Test a Service query
```
❯ ./hymenaios -config.file public.yml -test.service EXAMPLE/SERVICE
Loading config from "public.yml"
INFO: Testing (EXAMPLE/SERVICE)
INFO: EXAMPLE/SRVICE, Latest Release - "3.2.1"
```

## Help finding ID
You can go through your `config.yml` and find what ID you defined in the service/gotify/slack mapping, or you could give an undefined ID to this flag.

e.g.
```
❯ ./hymenaios -test.service a
Loading config from "config.yml"
INFO: Testing (a)
ERROR: Testing (a), Service "a" could not be found in config.service
Did you mean one of these?
  - EXAMPLE_1
  - EXAMPLE_2
  - EXAMPLE_3
```
