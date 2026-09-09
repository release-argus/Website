---
title: "Authentication"
linkTitle: "Authentication"
weight: 3
description: >
  User accounts, permissions and API tokens
---

Argus can require a login and govern every API route with per-user permissions. It is off by default - turn it on with [`settings.auth.enabled`](/docs/config/settings/#auth).

With auth off, nothing changes: the dashboard and API stay open (or behind `settings.web.basic_auth`, which cannot be combined with `settings.auth`).

## Getting in the first time

Until an account exists, the first-run setup page is reachable **without credentials**, and whoever completes it becomes the administrator. On an instance that is already reachable from an untrusted network, create the account before starting the server:

```bash
argus -auth.create-admin admin
```

It generates a password, prints it to stdout and exits without serving anything, so setup is closed before the first connection is accepted.

## Users and groups

Permissions are never granted to a user directly - a user belongs to groups, and groups hold the grants. Administrators manage both under **Admin → Users** and **Admin → Groups**.

Three groups are seeded:

| Group | Seeded with | Notes |
| --- | --- | --- |
| `admin` | Every permission | A system group. Cannot be deleted or renamed, and its grants are fixed - re-synced on every start, so newly added permissions are always held by someone. |
| `operator` | Everything except `metric:read` | Created on first install, then yours: rename, edit or delete it freely. |
| `viewer` | Every `read` action except `metric:read` | Same - a starting point, not a fixed role. |

`operator` and `viewer` are seeded once and never re-created or re-synced, so your edits survive restarts and upgrades.

Argus refuses to leave itself unadministered: the last enabled member of `admin` cannot be deleted, disabled, or removed from the group.

## Permissions

A grant is a **resource**, an **action**, and a **scope**.

| Resource | Actions | Scopes | Covers |
| --- | --- | --- | --- |
| `service` | `create` | global | Creating services. |
| `service` | `read`, `update`, `delete` | global, service, service_tag | Viewing and editing service configs. |
| `service_order` | `update` | global | Reordering the dashboard. |
| `service_action` | `execute` | global, service, service_tag | Approving/skipping releases, running webhooks and commands. |
| `version_refresh` | `execute` | global, service, service_tag | On-demand latest/deployed version refreshes. |
| `notify` | `execute` | global | The 'Send Test Message' button. |
| `config` | `read` | global | Viewing the full config. |
| `metric` | `read` | global | `/metrics` and the dashboard counts. |

Scopes narrow a grant to a subset of services:

- **global** - every service.
- **service** - one service, by its ID.
- **service_tag** - every service carrying a `dashboard.tags` entry.

A user with no `service:read` grant sees an empty dashboard rather than an error, and the WebSocket only delivers updates for services they may read.

{{< alert title="Note" >}}
Renaming a service moves any `service`-scoped grants pointing at it, so a rename never silently revokes someone's access. Deleting a service prunes them.
{{< /alert >}}

## Sessions

Sessions are cookie-based (`argus_session`, `HttpOnly`, `SameSite=Strict`) and bounded two ways: an absolute `lifetime` from login, and an `idle_timeout` sliding window refreshed by activity. Both are configurable - see [settings](/docs/config/settings/#auth).

Each user is capped at `session.max_per_user` concurrent sessions (10 by default); logging in past the cap evicts the least recently active one.

Changes take effect immediately rather than at the next login (permissions are re-read on every request). Changing a password, disabling an account, or editing a group's grants ends or updates the affected sessions straight away and disconnects their live dashboard connections. The one exception is changing your own password from [Settings](#your-own-account) - that session is re-issued, so you stay signed in where you made the change.

Failed logins are rate limited per (IP, username) and per IP. Behind a reverse proxy, set [`trusted_proxies`](/docs/config/settings/#trusted_proxies) so the limiter counts real client addresses rather than the proxy's.

## Your own account

Every signed-in user can edit their own account under **Settings → Account**, reached from the user menu, whatever permissions they hold. Three things are editable - display name, email, and password. Usernames are fixed at account creation.

Setting a new password signs out your **other** sessions and disconnects their dashboards, leaving only the one you changed it in. API tokens are untouched by it - see [losing access](#losing-access).

## API tokens

Users mint their own tokens under **Settings → API Tokens**, optionally with an expiry. The token is shown once, at creation, and stored only as a hash - if it is lost, revoke it and create another.

Use one as a bearer token:

```bash
curl -H "Authorization: Bearer argus_xxxxxxxx…" \
  https://argus.example.com/api/v1/service/summary
```

A token carries its owner's permissions. Two limits apply:

- Tokens cannot manage tokens or change their owner's account details (both need a browser session).
- Revoking a token is immediate, but a password reset does **not** revoke tokens.

### Scraping metrics

With auth on, `/metrics` requires `metric:read`. Give Prometheus a token that holds it - see
[Prometheus](/docs/monitor/prometheus/).

## Behind a reverse proxy

Set [`trusted_proxies`](/docs/config/settings/#trusted_proxies) to your proxy's addresses. Without it, `X-Forwarded-For` and `X-Forwarded-Host` are ignored and every request appears to come from the proxy - which means one failed-login flood locks out every user.

State-changing requests are checked against the request's origin, so the proxy must preserve the `Host` header (or send `X-Forwarded-Host`) for logins to succeed.

## Losing access

| Situation | Recovery |
| --- | --- |
| Forgotten password | `argus -auth.reset-password <username>` - generates a new password, prints it, and revokes that user's sessions. Their API tokens are left intact. |
| An account has been taken over | A password reset is not enough: their API tokens survive it. Delete the account, or its tokens, from another administrator's session. |
| No administrator reachable at all | Delete every row from the `users` table of `settings.data.database_file`. First-run setup reopens; sessions, memberships and tokens are removed with the accounts. |
