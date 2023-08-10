---
title: "Services"
linkTitle: "Services"
weight: 2
description: >
  Example configurations for different services.
---

## AdguardTeam/AdGuardHome
Source: https://github.com/AdguardTeam/AdGuardHome

- deployed_version - Requires `USERNAME` and `PASSWORD` Basic Auth credentials created during the setup wizard on first launch unless you explicitly disabled it.
```yaml
service:
  AdguardTeam/AdGuardHome:
    latest_version:
      type: github
      url: AdguardTeam/AdGuardHome
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://adguard.example.io/control/status
      basic_auth:
        username: <USERNAME>
        password: <PASSWORD>
      json: version
      regex: v([0-9.]+)
    dashboard:
      web_url: https://github.com/AdguardTeam/AdGuardHome/releases/v{{ version }}
      icon: https://avatars.githubusercontent.com/u/8361145?s=200&v=4
```

## adnanh/webhook
Source: https://github.com/adnanh/webhook
```yaml
service:
  adnanh/webhook:
    latest_version:
      type: github
      url: adnanh/webhook
      require:
        regex_content: webhook-linux-amd64\.tar\.gz
    dashboard:
      web_url: https://github.com/adnanh/webhook/releases/{{ version }}
      icon: https://raw.githubusercontent.com/adnanh/webhook/development/docs/logo/logo-128x128.png
```

## ansible/awx
Source: https://github.com/ansible/awx
```yaml
service:
  ansible/awx:
    latest_version:
      type: github
      url: ansible/awx
    deployed_version:
      url: https://awx.example.io/api/v2/ping/?format=json
      json: version
    dashboard:
      web_url: https://github.com/ansible/awx/releases/{{ version }}
      icon: https://raw.githubusercontent.com/ansible/awx-logos/master/awx/ui/client/assets/logo-login.svg
```

## ansible/awx-operator
Source: https://github.com/ansible/awx-operator
```yaml
service:
  ansible/awx-operator:
    latest_version:
      type: github
      url: ansible/awx-operator
    dashboard:
      web_url: https://github.com/ansible/awx-operator/releases/{{ version }}
      icon: https://raw.githubusercontent.com/ansible/awx-logos/master/awx/ui/client/assets/logo-login.svg
```

## argoproj/argo-cd
Source: https://github.com/argoproj/argo-cd
```yaml
service:
  argoproj/argo-cd:
    latest_version:
      type: github
      url: argoproj/argo-cd
      url_commands:
      - type: regex
        regex: v([0-9.]+)$
    deployed_version:
      url: https://argocd.example.io/api/version
      json: Version
      regex: v([0-9.]+)
    dashboard:
      web_url: https://github.com/argoproj/argo-cd/releases/v{{ version }}
      icon: https://avatars.githubusercontent.com/u/30269780?s=200&v=4
```

## dani-garcia/vaultwarden
Source: https://github.com/dani-garcia/vaultwarden

- deployed_version - Requires version `1.25.0` of Vaultwarden

```yaml
service:
  dani-garcia/vaultwarden:
    latest_version:
      type: github
      url: dani-garcia/vaultwarden
    deployed_version:
      url: https://vaultwarden.example.io/api/version
      regex: ([0-9.]+)
    dashboard:
      web_url: https://github.com/dani-garcia/vaultwarden/releases/{{ version }}
      icon: https://raw.githubusercontent.com/dani-garcia/vaultwarden/main/src/static/images/vaultwarden-icon.png
```

## Fallenbagel/jellyseerr
Source: https://github.com/Fallenbagel/jellyseerr
```yaml
service:
  Fallenbagel/jellyseerr:
    latest_version:
      type: github
      url: Fallenbagel/jellyseerr
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: http://jellyseerr.example.io/api/v1/status
      json: version
    dashboard:
      web_url: https://github.com/Fallenbagel/jellyseerr/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/Fallenbagel/jellyseerr/develop/public/os_icon.svg
```

## gitlab-org/gitlab
Source: https://gitlab.com/gitlab-org/gitlab

- To get the latest version of the 'Enterprise Edition', change the `ce` to `ee` in the `url_commands` `regex`.

- deployed_version - Requires an `Access_Token` from your GitLab instance. ([instructions](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html#create-a-personal-access-token))

```yaml
service:
  gitlab-org/gitlab:
    latest_version:
      type: url
      url: https://gitlab.com/api/v4/projects/278964/repository/tags
      url_commands:
        - type: regex
          regex: \"name\":\"v([0-9.]+\-ce)\"
    deployed_version:
      url: https://gitlab.example.com/api/v4/version
      headers:
        - key: PRIVATE-TOKEN
          value: <Access_Token>
      json: version
    dashboard:
      web_url: https://gitlab.com/gitlab-org/gitlab/-/blob/master/CHANGELOG.md
      icon: https://gitlab.com/gitlab-org/gitlab/-/raw/master/public/slash-command-logo.png
```

## go-gitea/gitea
Source: https://github.com/go-gitea/gitea:
```yaml
service:
  go-gitea/gitea:
    latest_version:
      type: github
      url: go-gitea/gitea
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
      require:
        regex_content: gitea-{{ version }}-linux-amd64
        regex_version: ^[0-9.]+[0-9]$
    deployed_version:
      url: https://git.example.io
      regex: 'Powered by Gitea\s+Version:\s+([0-9.]+) '
    dashboard:
      web_url: https://github.com/go-gitea/gitea/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/go-gitea/gitea/main/public/img/logo.png
```

## go-vikunja/api
Source: https://github.com/go-vikunja/api
```yaml
service:
  go-vikunja/api:
    latest_version:
      type: url
      url: https://github.com/go-vikunja/api/tags
      url_commands:
        - type: regex
          regex: \/releases\/tag\/v?([0-9.]+)\"
    deployed_version:
      url: https://vikunja.example.io/api/v1/info
      json: version
      regex: v?([0-9.]+)
    dashboard:
      web_url: https://github.com/go-vikunja/api/blob/main/CHANGELOG.md
      icon: https://vikunja.io/images/vikunja.png
```

## goauthentik/authentik
Source: https://github.com/goauthentik/authentik

- deployed_version - Requires an `API_Token` with API Access rights. (can be done at `Directory/Tokens & App password` / `/if/admin/#/core/tokens` as of '2022.4.1')
```yaml
service:
  goauthentik/authentik:
    latest_version:
      type: github
      url: goauthentik/authentik
      url_commands:
        - type: regex
          regex: version\/([0-9.]+[0-9]+(?:-rc[0-9])?)
    deployed_version:
      url: https://authentik.example.io/api/v3/admin/version/
      headers:
        - key: Authorization
          value: bearer <API_Token>
      json: version_current
    dashboard:
      web_url: https://goauthentik.io/docs/releases/{{ version | split:"." | slice:":-1" | join:"."  }}
      icon: https://raw.githubusercontent.com/goauthentik/authentik/master/web/icons/icon.png
```

## goharbor/harbor
Source: https://github.com/goharbor/harbor
```yaml
service:
  goharbor/harbor:
    latest_version:
      type: github
      url: goharbor/harbor
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://harbor.example.io/api/v2.0/systeminfo
      json: harbor_version
      regex: v([0-9.]+)
    dashboard:
      web_url: https://github.com/goharbor/harbor/releases/tag/v{{ version }}
      icon: https://github.com/goharbor/harbor/raw/main/src/portal/src/images/harbor-logo.svg
```

## gohugoio/hugo
Source: https://github.com/gohugoio/hugo
```yaml
service:
  gohugoio/hugo:
    latest_version:
      type: github
      url: gohugoio/hugo
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
      require:
        regex_content: hugo_{{ version }}_Linux-64bit\.deb
    dashboard:
      web_url: https://github.com/gohugoio/hugo/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/gohugoio/hugo/master/docs/static/img/hugo.png
```

## golang/go
Source: https://github.com/golang/go
```yaml
service:
  golang/go:
    options:
      semantic_versioning: false
    latest_version:
      type: url
      url: https://golang.org/dl/
      url_commands:
        - type: regex
          regex: go([0-9.]+[0-9]+)\.src\.tar\.gz
    dashboard:
      web_url: https://go.dev/doc/go{{ version | split:"." | slice:":2" | join:"."  }}
      icon: https://go.dev/images/gophers/motorcycle.svg
```

## gotify/server
Source: https://github.com/gotify/server
```yaml
service:
  gotify/server:
    latest_version:
      type: github
      url: gotify/server
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://gotify.example.io/version
      json: version
    dashboard:
      web_url: https://github.com/gotify/server/releases/v{{ version }}
      icon: https://github.com/gotify/logo/raw/master/gotify-logo.png
```

## grafana/grafana
Source: https://github.com/grafana/grafana
```yaml
service:
  grafana/grafana:
    latest_version:
      type: github
      url: grafana/grafana
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://grafana.example.io/login
      regex: v([0-9.]+)\s\([0-9a-z]+\)
    dashboard:
      web_url: https://github.com/grafana/grafana/releases/tag/v{{ version }}
      icon: https://cdn.worldvectorlogo.com/logos/grafana.svg
```

## grafana/loki
Source: https://github.com/grafana/loki
```yaml
service:
  grafana/loki:
    latest_version:
      type: github
      url: grafana/loki
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://loki.example.io/loki/api/v1/status/buildinfo
      json: version
    dashboard:
      web_url: https://grafana.com/docs/loki/latest/release-notes/v{{ version | split:"." | slice:":2" | join:"-"  }}
      icon: https://grafana.com/static/assets/img/blog/loki.png
```

## hashicorp/vault
Source: https://github.com/hashicorp/vault
```yaml
service:
  hashicorp/vault:
    latest_version:
      type: github
      url: hashicorp/vault
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://vault.example.io/v1/sys/health
      json: version
    dashboard:
      web_url: https://github.com/hashicorp/vault/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/hashicorp/vault/main/ui/public/vault-logo.svg
```

## healthchecks/healthchecks
Source: https://github.com/healthchecks/healthchecks
```yaml
service:
  healthchecks/healthchecks:
    options:
      semantic_versioning: false
    latest_version:
      type: github
      url: healthchecks/healthchecks
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://healthchecks.example.io/docs/
      regex: Healthchecks v([0-9.]+)
    dashboard:
      web_url: https://github.com/healthchecks/healthchecks/releases/tag/{{ version }}
      icon: https://healthchecks.io/static/img/logo-rounded.svg
```

## hedgedoc/hedgedoc
Source: https://github.com/hedgedoc/hedgedoc
```yaml
service:
  hedgedoc/hedgedoc:
    latest_version:
      type: github
      url: hedgedoc/hedgedoc
      require:
        regex_version: ^[0-9.]+$
    deployed_version:
      url: https://hedgedoc.example.io/config
      regex: window.version = '([0-9.]+)-[0-9a-z]+'
    dashboard:
      web_url: https://github.com/hedgedoc/hedgedoc/releases/tag/{{ version }}
      icon: https://raw.githubusercontent.com/hedgedoc/hedgedoc/master/public/icons/android-chrome-512x512.png
```

## home-assistant/core
Source: https://github.com/home-assistant/core

- deployed_version - `API_Token` obtained by navigating to  '/profile' and creating a 'Long-Lived Access Token'. ([instructions](https://developers.home-assistant.io/docs/auth_api/#long-lived-access-token))
```yaml
service:
  home-assistant/core:
    latest_version:
      type: github
      url: home-assistant/core
      require:
        regex_version: ^[0-9.]+$
    deployed_version:
      url: https://home-assistant.example.io/api/config
      json: version
      headers:
        - key: Authorization
          value: Bearer <API_Token>
    dashboard:
      web_url: https://www.home-assistant.io/latest-release-notes
      icon: https://github.com/home-assistant/core/raw/dev/tests/components/image_upload/logo.png
```

## influxdata/influxdb
Source: https://github.com/influxdata/influxdb
```yaml
service:
  influxdata/influxdb:
    latest_version:
      type: github
      url: influxdata/influxdb
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://influxdb.example.io/health
      json: version
    dashboard:
      web_url: https://github.com/influxdata/influxdb/releases/tag/v{{ version }}
      icon: https://github.com/influxdata/ui/raw/master/src/writeData/graphics/influxdb.svg
```

## jellyfin/jellyfin
Source: https://github.com/jellyfin/jellyfin
```yaml
service:
  jellyfin/jellyfin:
    latest_version:
      type: github
      url: jellyfin/jellyfin
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://jellyfin.example.io/System/Info/Public
      json: Version
    dashboard:
      web_url: https://github.com/jellyfin/jellyfin/releases/v{{ version }}
      icon: https://avatars.githubusercontent.com/u/45698031?s=200&v=4
```

## jgraph/drawio
Source: https://github.com/jgraph/drawio
```yaml
service:
  jgraph/drawio:
    latest_version:
      type: github
      url: jgraph/drawio
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://draw.example.io/package.json
      json: version
    dashboard:
      web_url: https://github.com/jgraph/drawio/releases/v{{ version }}
      icon: https://github.com/jgraph/drawio/raw/dev/src/main/webapp/images/drawlogo-color.svg
```

## louislam/uptime-kuma
Source: https://github.com/louislam/uptime-kuma
```yaml
service:
  louislam/uptime-kuma:
    latest_version:
      type: github
      url: louislam/uptime-kuma
    deployed_version:
      url: https://status.example.io/metrics
      regex: app_version{version=\"([0-9.]+)\",major=\"[0-9]+\",minor=\"[0-9]+\",patch=\"[0-9]+\"}
    dashboard:
      web_url: https://github.com/louislam/uptime-kuma/releases/{{ version }}
      icon: https://raw.githubusercontent.com/louislam/uptime-kuma/master/public/icon.png
```

## mailcow/mailcow-dockerized
Source: https://github.com/mailcow/mailcow-dockerized
```yaml
service:
  mailcow/mailcow-dockerized:
    options:
      semantic_versioning: false
    latest_version:
      type: github
      url: mailcow/mailcow-dockerized
      require:
        regex_version: ^[0-9-]+[a-z]?$
    deployed_version:
      url: https://mailcow.example.io/api/v1/get/status/version
      headers:
        - key: X-API-Key
          value: <ReadOnly-API-Key>
      json: version
    dashboard:
      web_url: https://github.com/mailcow/mailcow-dockerized/releases/tag/{{ version }}
      icon: https://raw.githubusercontent.com/mailcow/mailcow-dockerized/master/data/web/img/cow_mailcow.svg
```

## matomo-org/matomo
Source: https://github.com/matomo-org/matomo

- deployed_version - `TOKEN` can be generated in the personal settings under security. ([instructions](https://matomo.org/faq/general/faq_114/))
```yaml
service:
  matomo-org/matomo:
    latest_version:
      type: github
      url: matomo-org/matomo
      require:
        regex_version: ^[0-9.]+$
    deployed_version:
      url: https://matomo.example.io/index.php?module=API&method=API.getMatomoVersion&format=JSON&force_api_session=1&token_auth=<TOKEN>
      json: value
    dashboard:
      web_url: https://github.com/matomo-org/matomo/releases/tag/{{ version }}
      icon: https://raw.githubusercontent.com/matomo-org/matomo/4.x-dev/plugins/CoreHome/images/applogo_732.png
```

## matrix-org/synapse
Source: https://github.com/matrix-org/synapse
```yaml
service:
  matrix-org/synapse:
    latest_version:
      type: github
      url: matrix-org/synapse
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://matrix.example.io/_synapse/admin/v1/server_version
      json: server_version
    dashboard:
      web_url: https://github.com/matrix-org/synapse/releases/tag/v{{ version }}
      icon: https://github.com/matrix-org/synapse/raw/develop/docs/favicon.svg
```

## mattermost/mattermost-server
Source: https://github.com/mattermost/mattermost-server
```yaml
service:
  mattermost/mattermost-server:
    latest_version:
      type: url
      url: https://mattermost.com/deploy/
      url_commands:
        - type: regex
          regex: releases\.mattermost\.com\/([^\/]+)\/mattermost-[0-9.]+-linux\-amd64\.tar\.gz
    dashboard:
      web_url: https://docs.mattermost.com/install/self-managed-changelog.html
```

## morpheus65535/bazarr
Source: https://github.com/morpheus65535/bazarr

- deployed_version - Requires an `API_KEY` which can be retrieved at `Settings/General/Security/API Key`
```yaml
service:
  morpheus65535/bazarr:
    latest_version:
      type: github
      url: morpheus65535/bazarr
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://bazarr.example.io/api/system/status
      headers:
        - key: X-API-KEY
          value: <API_KEY>
      json: data.bazarr_version
    dashboard:
      web_url: https://github.com/morpheus65535/bazarr/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/morpheus65535/bazarr/master/frontend/public/images/logo128.png
```

## n8n-io/n8n
Source: https://github.com/n8n-io/n8n
```yaml
service:
  n8n-io/n8n:
    latest_version:
      type: url
      url: https://github.com/n8n-io/n8n/tags
      url_commands:
        - type: regex
          regex: n8n\%40([0-9.]+)
    dashboard:
      web_url: https://github.com/n8n-io/n8n/blob/master/CHANGELOG.md
      icon: https://raw.githubusercontent.com/n8n-io/n8n-docs/main/docs/_images/n8n-docs-icon.svg
```

## netbox-community/netbox
Source: https://github.com/netbox-community/netbox
```yaml
service:
  netbox-community/netbox:
    latest_version:
      type: github
      url: netbox-community/netbox
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
      require:
        regex_version: ^[0-9.]+$
    deployed_version:
      url: https://netbox.example.io/api/status/
      json: netbox-version
    dashboard:
      web_url: https://github.com/netbox-community/netbox/releases/tag/v{{ version }}
      icon: https://github.com/netbox-community/netbox/raw/develop/netbox/project-static/img/netbox_icon.svg
```

## nextcloud/server
Source: https://github.com/nextcloud/server
```yaml
service:
  nextcloud/server:
    latest_version:
      type: github
      url: nextcloud/server
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://nextcloud.example.io/status.php
      json: versionstring
    dashboard:
      web_url: https://nextcloud.com/changelog/#latest{{ version | split:"." | slice:":1" | join:""  }}
      icon: https://github.com/nextcloud/server/raw/master/core/img/favicon.png
```

## opencve/opencve
Source: https://github.com/opencve/opencve
```yaml
service:
  opencve/opencve:
    latest_version:
      type: github
      url: opencve/opencve
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    dashboard:
      web_url: https://github.com/opencve/opencve/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/opencve/opencve/master/opencve/static/img/logo_white.png
```

## opnsense/core
Source: https://github.com/opnsense/core

- deployed_version - Create an API Key. ([instructions](https://docs.opnsense.org/development/how-tos/api.html#creating-keys))
```yaml
service:
  opnsense/core:
    latest_version:
      type: url
      url: https://github.com/opnsense/core/tags
      url_commands:
        - type: regex
          regex: \/releases\/tag\/([0-9.]+)\"
    deployed_version:
      url: https://opnsense.example.io/api/core/firmware/status
      basic_auth:
        username: <API Key>
        password: <API Secret>
      json: product.product_version
      regex: ([0-9.]+)
    dashboard:
      web_url: https://docs.opnsense.org/CE_releases.html
      icon: https://github.com/opnsense/core/raw/master/src/opnsense/www/themes/opnsense/build/images/icon-logo.svg
```

## prometheus/alertmanager
Source: https://github.com/prometheus/alertmanager
```yaml
service:
  prometheus/alertmanager:
    latest_version:
      type: github
      url: prometheus/alertmanager
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://alertmanager.example.io/api/v1/status
      json: data.versionInfo.version
    dashboard:
      web_url: https://github.com/prometheus/alertmanager/blob/main/CHANGELOG.md
      regex_content: alertmanager-{{ version }}\.linux-amd64.tar.gz
```

## prometheus/prometheus
Source: https://github.com/prometheus/prometheus
```yaml
service:
  prometheus/prometheus:
    latest_version:
      type: github
      url: prometheus/prometheus
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://prometheus.example.io/api/v1/status/buildinfo
      json: data.version
    dashboard:
      web_url: https://github.com/prometheus/prometheus/blob/main/CHANGELOG.md
      regex_content: prometheus-{{ version }}\.linux-amd64
```

## Prowlarr/Prowlarr
Source: https://github.com/Prowlarr/Prowlarr

- deployed_version - Requires an `API_KEY` which can be retrieved at `Settings/General/Security/API Key`
```yaml
service:
  Prowlarr/Prowlarr:
    options:
      semantic_versioning: false
    latest_version:
      type: github
      url: Prowlarr/Prowlarr
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
      use_prerelease: true
    deployed_version:
      url: https://prowlarr.example.io/api/v1/system/status
      headers:
        - key: X-Api-Key
          value: <API_KEY>
      json: version
    dashboard:
      web_url: https://github.com/Prowlarr/Prowlarr/releases/v{{ version }}
      icon: https://avatars.githubusercontent.com/u/73049443?s=200&v=4
```

## pterodactyl/panel
Source: https://github.com/pterodactyl/panel
```yaml
service:
  pterodactyl/panel:
    latest_version:
      type: github
      url: pterodactyl/panel
      url_commands:
        - type: regex
          regex: v([0-9.]+)
    dashboard:
      web_url: https://github.com/pterodactyl/panel/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/pterodactyl/panel/develop/public/assets/svgs/pterodactyl.svg
```

## pterodactyl/wings
Source: https://github.com/pterodactyl/wings

- deployed_version - Needs the node token which can be found in the admin GUI in the node configuration. ([instructions](https://dashflo.net/docs/api/pterodactyl/v1/#authentication))
```yaml
service:
  pterodactyl/wings:
    latest_version:
      type: github
      url: pterodactyl/wings
      url_commands:
        - type: regex
          regex: v([0-9.]+)
    deployed_version:
      url: https://wings.example.io:8088/api/system
      headers:
        - key: Authorization
          value: Bearer <API Token>
      json: version
    dashboard:
      web_url: https://github.com/pterodactyl/wings/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/pterodactyl/panel/develop/public/assets/svgs/pterodactyl.svg
```

## Radarr/Radarr
Source: https://github.com/Radarr/Radarr

- deployed_version - Requires an `API_KEY` which can be retrieved at `Settings/General/Security/API Key`
```yaml
service:
  Radarr/Radarr:
    options:
      semantic_versioning: false
    latest_version:
      type: github
      url: Radarr/Radarr
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://radarr.example.io/api/v3/system/status
      headers:
        - key: X-Api-Key
          value: <API_KEY>
      json: version
    dashboard:
      web_url: https://github.com/Radarr/Radarr/releases/v{{ version }}
      icon: https://avatars.githubusercontent.com/u/25025331?s=200&v=4
```

## rancher/rancher
Source: https://github.com/rancher/rancher

- deployed_version - An API key must be created. This key is constructed in the format of `<username>:<password>`. ([instructions](https://rancher.com/docs/rancher/v2.5/en/user-settings/api-keys/#creating-an-api-key))
```yaml
service:
  rancher/rancher:
    latest_version:
      type: github
      url: rancher/rancher
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://rancher.example.io/v3/settings/server-version
      basic_auth:
        username: <username>
        password: <password>
      json: value
      regex: v([0-9.]+)
    dashboard:
      web_url: https://github.com/rancher/rancher/releases/v{{ version }}
      icon: https://raw.githubusercontent.com/rancher/docs/master/static/imgs/rancher-logo-cow-blue.svg
```

## release-argus/argus
Source: https://github.com/release-argus/Argus
```yaml
service:
  release-argus/argus:
    latest_version:
      type: github
      url: release-argus/argus
    deployed_version:
      url: https://argus.example.io/api/v1/version
      json: version
    dashboard:
      web_url: https://github.com/release-argus/Argus/blob/master/CHANGELOG.md
      icon: https://github.com/release-argus/Argus/raw/master/web/ui/static/favicon.svg
```

## requarks/wiki
Source: https://github.com/requarks/wiki
```yaml
service:
  requarks/wiki:
    latest_version:
      type: github
      url: requarks/wiki
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
      require:
        regex_version: ^[0-9.]+$
    deployed_version:
      url: https://wiki.example.io/graphql?query=%7Bsystem%7Binfo%7BcurrentVersion%7D%7D%7D
      headers:
        - key: Authorization
          value: Bearer <TOKEN>
      json: data.system.info.currentVersion
    dashboard:
      web_url: https://github.com/requarks/wiki/releases/tag/v{{ version }}
      icon: https://static.requarks.io/logo/wikijs-butterfly.svg
```

## smallstep/certificates
Source: https://github.com/smallstep/certificates
```yaml
service:
  smallstep/certificates:
    latest_version:
      type: github
      url: smallstep/certificates
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://certificates.example.io/version
      json: version
    dashboard:
      web_url: https://github.com/smallstep/certificates/releases/tag/v{{ version }}
      icon: https://github.com/smallstep/docs/raw/main/static/graphics/logo-icon-white.svg
```

## Sonarr/Sonarr
Source: https://github.com/Sonarr/Sonarr

- deployed_version - Requires an `API_KEY` which can be retrieved at `Settings/General/Security/API Key`
```yaml
service:
  Sonarr/Sonarr:
    options:
      semantic_versioning: false
    latest_version:
      type: url
      url: https://github.com/Sonarr/Sonarr/tags
      url_commands:
        - type: regex
          regex: \/releases\/tag\/v?([0-9.]+)\"
    deployed_version:
      url: https://sonarr.example.io/api/v3/system/status
      headers:
        - key: X-Api-Key
          value: <API_KEY>
      json: version
    dashboard:
      web_url: https://sonarr.example.io/system/updates
      icon: https://raw.githubusercontent.com/Sonarr/Sonarr/develop/Logo/256.png
```

## thanos-io/thanos
Source: https://github.com/thanos-io/thanos
```yaml
service:
  thanos-io/thanos:
    latest_version:
      type: github
      url: thanos-io/thanos
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
      require:
        regex_content: thanos-{{ version }}\.linux-amd64
    deployed_version:
      url: https://thanos.example.io/api/v1/status/buildinfo
      json: data.version
    dashboard:
      web_url: https://github.com/thanos-io/thanos/blob/main/CHANGELOG.md
      icon: https://github.com/thanos-io/thanos/blob/main/docs/img/Thanos-logo_fullmedium.png?raw=true
```

## vector-im/element-web
Source: https://github.com/vector-im/element-web
```yaml
service:
  vector-im/element-web:
    latest_version:
      type: github
      url: vector-im/element-web
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    deployed_version:
      url: https://element.example.io/version
    dashboard:
      web_url: https://github.com/vector-im/element-web/releases/tag/v{{ version }}
      icon: https://github.com/vector-im/element-web/raw/develop/res/vector-icons/150.png
```

## wekan/wekan
Source: https://github.com/wekan/wekan
```yaml
service:
  wekan/wekan:
    options:
      semantic_versioning: false
    latest_version:
      type: github
      url: wekan/wekan
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
      require:
        regex_version: ^[0-9.]+$
    dashboard:
      web_url: https://github.com/wekan/wekan/releases/tag/v{{ version }}
      icon: https://raw.githubusercontent.com/wekan/wekan/df54863e7243b0b067ec2d30d8352ff1838931c4/meta/icons/wekan-150.svg
```

## wordpress/wordpress
Source: https://github.com/WordPress/WordPress
```yaml
service:
  wordpress/wordpress:
    latest_version:
      type: url
      url: https://github.com/wordpress/wordpress/tags
      url_commands:
        - type: regex
          regex: \/releases\/tag\/([0-9.]+)\"
    deployed_version:
      url: https://wordpress.example.io/feed/
      regex: '<generator>https:\/\/wordpress\.org\/\?v=([0-9.]*)\<\/generator\>'
    dashboard:
      web_url: https://wordpress.org/news/category/releases/
      icon: https://github.com/WordPress/WordPress/raw/master/wp-admin/images/wordpress-logo.svg
```

## wowchemy/wowchemy-hugo-themes
Source: https://github.com/wowchemy/wowchemy-hugo-themes
```yaml
service:
  wowchemy/wowchemy-hugo-themes:
    latest_version:
      type: github
      url: wowchemy/wowchemy-hugo-themes
      url_commands:
        - type: regex
          regex: v([0-9.]+)$
    dashboard:
      web_url: https://github.com/wowchemy/wowchemy-hugo-themes/releases/v{{ version }}
```
