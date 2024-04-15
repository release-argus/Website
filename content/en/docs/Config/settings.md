---
title: "Settings"
linkTitle: "Settings"
weight: 1
description: >
  Settings to run the binary with, e.g. listen host/port, https details, log level.
---

Below are the options available in their default state. These values can be set with environment variables in the format of `ARGUS_<YAML_PATH_UNDER_SETTINGS>`. For example, `ARGUS_DATA_DATABASE_FILE=/opt/argus.db` would set the default database file location to `/opt/argus.db` (`settings.data.database_file`).

{{< alert title="Note" >}}
Environment variables in the format ${ENV_VAR} can be used in the `settings.web.basic_auth.password` and `settings.web.basic_auth.username` fields.
{{< /alert >}}

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
    disabled_routes: []   # API Routes to disable (see below)
    favicon:
      png: ''  # Override /apple-touch-icon.png (e.g. https://example.com/apple-touch-icon.png)
      svg: ''  # Override /favicon.svg (e.g. https://example.com/favicon.svg)
```


## web

### disabled_routes
Specific API routes can be disabled through the `config.yml` file located at `settings.web.disabled_routes`. These routes are associated with particular functionalities, and below is a summary of what each one does:
```yaml
settings:
  web:
    disabled_routes:
      - service_create  # Creation of new services
      - service_update  # Updating of existing services
      - service_delete  # Deletion of services
      - notify_test     # Testing of Notify's (via the 'Send Test Message' button)
      - lv_refresh      # Manually refreshing the latest version
      - dv_refresh      # Manually refreshing the deployed version
      - lv_refresh_new  # Manually refreshing the latest version of uncreated services
      - dv_refresh_new  # Manually refreshing the deployed version of uncreated services
      - service_actions # Approving/skipping releases
```
