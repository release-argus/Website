---
title: "Prometheus"
linkTitle: "Prometheus"
weight: 4
description: >
  Argus monitoring with Prometheus
---

Argus exports Prometheus metrics on the `/metrics` endpoint. You can use the Prometheus exporter to monitor your Argus instance.

## List of metrics

Additional to the default Prometheus Go library metrics (refer to [prometheus/client_golang](https://github.com/prometheus/client_golang)), Argus exports the following metrics.

| Metric | Description |
| --- | --- |
| `command_result_total` | Number of commands executed. |
| `deployed_version_query_result_last` | Last deployed version query was successful(`0`=no,`1`=yes). |
| `deployed_version_query_result_total` | Number of deployed version checks. |
| `latest_version_is_deployed` | Latest version is the same as its deployed version (`0`=no, `1`=yes, `2`=approved, `3`=skipped) |
| `latest_version_query_result_last` | Last latest version query was successful (`0`=no, `1`=yes, `2`=no_regex_match, `3`=semantic_version_fail, `4`=progressive_version_fail). |
| `latest_version_query_result_total` | Number of latest version checks. |
| `notify_result_total` | Number of notifications sent. |
| `webhook_result_total` | Number of webhooks sent. |
