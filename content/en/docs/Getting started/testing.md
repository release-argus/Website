---
title: "Testing"
linkTitle: "Testing"
weight: 5
description: >
  Testing your config
---

## Test a Notify connection
```bash
❯ ./argus -test.notify EXAMPLE_NOTIFY
Loading config from "config.yml"
INFO: Testing (EXAMPLE_NOTIFY)
INFO: Message sent successfully with "EXAMPLE_NOTIFY" config
```

## Test a Service query
```bash
❯ ./argus -config.file public.yml -test.service EXAMPLE/SERVICE
Loading config from "public.yml"
INFO: Testing (EXAMPLE/SERVICE)
INFO: EXAMPLE/SERVICE, Latest Release - "3.2.1"
```

## Help finding ID
You can go through your `config.yml` and find what ID you defined in the service/notify mapping, or you could give an undefined ID to this flag.

e.g.
```bash
❯ ./argus -test.service a
Loading config from "config.yml"
INFO: Testing (a)
ERROR: Testing (a), Service "a" could not be found in config.service
Did you mean one of these?
  - EXAMPLE_1
  - EXAMPLE_2
  - EXAMPLE_3
```
