---
title: "Settings"
linkTitle: "Settings"
weight: 1
description: >
  Settings to run the binary with, e.g. listen host/port, https details, log level.
---

Below are the options available in their default state.

config.yml:
```yaml
settings:
  log:
    level: INFO       # Log level, DEBUG/VERBOSE/INFO/WARNING/ERROR
    timestamps: false # Log with timestamps
  data:
    database_file: data/argus.db # SQLite DB file used to track the state of services
  web:
    listen_host: 0.0.0.0  # IP address to listen on
    listen_port: 8080     # Port to listen on
    route_prefix: /       # Web route prefix, e.g. /demo means http://IP:PORT/demo to access
    cert_file: ''         # HTTPS Cert path, e.g. `cert.pem`
    pkey_file: ''         # HTTPS PrivKey path, e.g. `privkey.pem`
    basic_auth:
      username: ''        # Basic auth username, e.g. `admin`
      password: ''        # Basic auth password, e.g. `test123`
```
