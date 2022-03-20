---
#categories: ["Examples", "Placeholders"]
#tags: ["test","docs"] 
title: "Getting Started"
linkTitle: "Getting Started"
weight: 2
description: >
  Deploying Hymenaios
---

Welcome to Hymenaios! Hymenaios is a software release monitor that can notify and act on new releases that match the filters you set. This guide will show you how to install, configure and monitor your first resource with Hymenaios. You'll download, install and run Hymenaios.

### Installation

[Download the latest release](https://github.com/hymenaios-io/Hymenaios/releases) of Hymenaios for your platform.

The Hymenaios server is a single binary named in the style of `hymenaios-VERSION.PLATFORM.ARCH`. We can run this binary and view help on its options by passing the `-help` flag.
```
❯ ./hymenaios-0.0.0.linux-amd64 -help
Usage of ./hymenaios-0.0.0.linux-amd64:
  -config.check
        Print the fully-parsed config.
...
```
Before starting Hymenaios, let's give it something to monitor.

### Config

To configure the `config.yml`, follow the various guidance [here](/docs/config).

### Try it out!

Test the demo [here!](/demo/approvals)
