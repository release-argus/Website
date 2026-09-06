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
  auth:
    enabled: false          # Enable user accounts and RBAC
    local:
      enabled: true         # Enable the local (username/password) provider
    session:
      lifetime: 720h        # Absolute cap on a session, from login
      idle_timeout: 168h    # Sliding window, refreshed by activity
      max_per_user: 10      # Concurrent sessions per user (oldest evicted first)
      #secure_cookie: false # Force the session cookie's 'Secure' attribute
                            # (auto-detected from HTTPS/X-Forwarded-Proto otherwise)
  log:
    level: INFO       # Log level, DEBUG/VERBOSE/INFO/WARNING/ERROR
    timestamps: false # Log with timestamps
  data:
    database_file: data/argus.db # SQLite DB file used to track the state of services
    readonly: false              # Disable writes to the config.yml
                                 # (create/edit/delete on the UI will not persist restarts)
  web:
    listen_host: 0.0.0.0  # IP address to listen on
    listen_port: 8080     # Port to listen on
    route_prefix: /       # Web route prefix, e.g. /demo means http://IP:PORT/demo to access
    cert_file: ''         # HTTPS Cert path, e.g. `cert.pem`
    pkey_file: ''         # HTTPS PrivKey path, e.g. `privkey.pem`
    basic_auth:
      username: ''        # Basic auth username, e.g. `admin`
      password: ''        # Basic auth password, e.g. `test123`
    disabled_routes: []   # API Routes to disable
    trusted_proxies: []   # Proxy IPs/CIDRs whose forwarded headers are trusted
    favicon:
      png: ''  # Override /apple-touch-icon.png (e.g. https://example.com/apple-touch-icon.png)
      svg: ''  # Override /favicon.svg (e.g. https://example.com/favicon.svg)
```


## auth

With `settings.auth.enabled: true`, Argus serves a login page and every API route is governed by per-user RBAC grants.

{{< alert title="Note" >}}
`settings.auth` and `settings.web.basic_auth` are mutually exclusive, pick one.
{{< /alert >}}

| Field | Default | Description |
| --- | --- | --- |
| `enabled` | `false` | Enable user accounts and RBAC. |
| `local.enabled` | `true` | The username/password provider. At least one provider must be enabled. |
| `session.lifetime` | `720h` | Absolute cap on a session, measured from login. Not extended by activity. |
| `session.idle_timeout` | `168h` | Sliding window, refreshed by activity. Must not exceed `lifetime`. |
| `session.max_per_user` | `10` | Concurrent sessions per user. Logging in beyond the cap evicts the least recently active. |
| `session.secure_cookie` | auto | Forces the session cookie's `Secure` attribute. Detected from HTTPS and `X-Forwarded-Proto: https` otherwise. |

Durations use Go's format (`720h`, `30m`, `1h30m`).

### The first administrator

Until an account exists, the first-run setup page is reachable **without credentials** (the first visitor to complete it becomes the administrator). Either complete setup before exposing a freshly auth-enabled instance to an untrusted network, or create the account up front:

```bash
argus -auth.create-admin <username>
```

That generates a password, prints it to stdout and exits, so setup is already closed by the time the server accepts connections. It only creates the *first* account (once any user exists it fails).

### Recovering access

```bash
argus -auth.reset-password <username>
```

Generates a new password for an existing user, prints it to stdout and revokes their sessions.

{{< alert title="Note" >}}
A password reset does **not** revoke that user's API tokens.
{{< /alert >}}

### Permissions

See [Authentication](/docs/help/authentication/) for the permission catalogue, the built-in groups, API tokens and reverse-proxy notes.


## web

### trusted_proxies
Behind a reverse proxy, list the proxy's IP addresses or CIDR ranges so Argus knows whose forwarded headers to believe:
```yaml
settings:
  web:
    trusted_proxies:
      - 10.0.0.5       # A single address
      - 192.168.0.0/16 # A range
      - ::1
```
`X-Forwarded-For` and `X-Forwarded-Host` are honoured only when the request arrives from one of these peers; otherwise the connecting address is used. This governs the client IP that appears in logs and that login rate limiting counts against, so leaving it unset behind a proxy makes every request look like it came from the proxy (one user exhausting the login limiter would lock out everyone).

`X-Forwarded-Proto: https` is honoured from any peer, so the session cookie is marked `Secure` even before `trusted_proxies` is set. A proxy that omits that header needs `settings.auth.session.secure_cookie: true`.


### disabled_routes
Specific API routes can be disabled through the `config.yml` file located at `settings.web.disabled_routes`. These routes are associated with particular functionalities, and below is a summary of what each one does:
```yaml
settings:
  web:
    disabled_routes:
      - order_edit      # Editing of service order
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
