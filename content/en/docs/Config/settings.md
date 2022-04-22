---
title: "Settings"
linkTitle: "Settings"
weight: 1
description: >
  Settings to run the binary with, e.g. listen address/port, https details, log level.
---

Below are the options available in their default state.

config.yml:
```yaml
settings:
  log:
    level: INFO              # Log level, DEBUG/VERBOSE/INFO/WARNING/ERROR
    timestamps: false        # Log with timestamps
  web:
    listen_address: 0.0.0.0  # Address to listen on
    listen_port: 8080        # Port to listen on
    route_prefix: /          # Web route prefix, e.g. /demo means http://IP:PORT/demo to access
    cert_file: ''            # HTTPS Cert path, e.g. cert.pem
    pkey_file: ''            # HTTPS PrivKey path, e.g. privkey.pem
```
