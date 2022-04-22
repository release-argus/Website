---
title: "Services"
linkTitle: "Services"
weight: 2
description: >
  Example configurations for different services.
---
## adnanh/webhook
Source: https://github.com/adnanh/webhook
```yaml
service:
  adnanh/webhook:
    type: github
    url: adnanh/webhook
    web_url: https://github.com/adnanh/webhook/releases/{{ version }}
    regex_content: webhook-linux-amd64\.tar\.gz
    icon: https://raw.githubusercontent.com/adnanh/webhook/development/docs/logo/logo-128x128.png
```

## ansible/awx-operator
Source: https://github.com/ansible/awx-operator
```yaml
service:
  ansible/awx-operator:
    type: github
    url: ansible/awx-operator
    web_url: https://github.com/ansible/awx-operator/releases/{{ version }}
    icon: https://raw.githubusercontent.com/ansible/awx-logos/master/awx/ui/client/assets/logo-login.svg
```

## dani-garcia/vaultwarden
Source: https://github.com/dani-garcia/vaultwarden
```yaml
service:
  dani-garcia/vaultwarden:
    type: github
    url: dani-garcia/vaultwarden
    web_url: https://github.com/dani-garcia/vaultwarden/releases/{{ version }}
    icon: https://raw.githubusercontent.com/dani-garcia/vaultwarden/main/src/static/images/vaultwarden-icon.png
```

## jgraph/drawio
Source: https://github.com/jgraph/drawio
```yaml
service:
  jgraph/drawio:
    type: github
    url: jgraph/drawio
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/jgraph/drawio/releases/v{{ version }}
    icon: https://github.com/jgraph/drawio/raw/dev/src/main/webapp/images/drawlogo-color.svg
    deployed_version:
      url: https://draw.example.io/package.json
      json: version
```

## gitlab-org/gitlab
Source: https://gitlab.com/gitlab-org/gitlab

You need a Access Token from your GitLab instance to get the current version.

To get the version from a Enterprise Edition change the `ce` to `ee` in the `url_commands` `regex`.
```yaml
service:
  gitlab-org/gitlab:
    type: url
    url: https://gitlab.com/api/v4/projects/278964/repository/tags
    url_commands:
      - type: regex_submatch
        regex: \"name\":\"v([0-9.]+\-ce)\"
    web_url: https://gitlab.com/gitlab-org/gitlab/-/blob/master/CHANGELOG.md
    icon: https://gitlab.com/gitlab-org/gitlab/-/raw/master/public/slash-command-logo.png
    deployed_version:
      url: https://gitlab.example.com/api/v4/version
      headers:
        - key: PRIVATE-TOKEN
          value: <Access-Token Own GitLab>
      json: version
```

## go-gitea/gitea
Source: https://github.com/go-gitea/gitea:
```yaml
service:
  go-gitea/gitea:
    type: github
    url: go-gitea/gitea
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/go-gitea/gitea/releases/v{{ version }}
    regex_content: gitea-{{ version }}-linux-amd64
    regex_version: ^[0-9.]+[0-9]$
    icon: https://raw.githubusercontent.com/go-gitea/gitea/main/public/img/logo.png
    deployed_version:
      url: https://git.example.io
      regex: 'Powered by Gitea Version: ([0-9.]+) '
```

## go-vikunja/api
Source: https://github.com/go-vikunja/api
```yaml
service:
  go-vikunja/api:
    type: url
    url: https://github.com/go-vikunja/api/tags
    url_commands:
      - type: regex_submatch
        regex: \/releases\/tag\/v?([0-9.]+)\"
    web_url: https://github.com/go-vikunja/api/blob/main/CHANGELOG.md
    icon: https://vikunja.io/images/vikunja.png
    deployed_version:
      url: https://vikunja.example.io/api/v1/info
      json: version
      regex: v?([0-9.]+)
```

## goauthentik/authentik
Source: https://github.com/goauthentik/authentik

To get the deployed version from authentik you must create a API Token with API Access rights.
```yaml
service:
  goauthentik/authentik:
    type: github
    url: goauthentik/authentik
    url_commands:
      - type: regex_submatch
        regex: version\/([0-9.]+[0-9]+(?:-rc[0-9])?)
    web_url: https://goauthentik.io/docs/releases/{{ version | split:"." | slice:":-1" | join:"."  }}
    icon: https://raw.githubusercontent.com/goauthentik/authentik/master/web/icons/icon.png
    deployed_version:
      url: https://authentik.example.io/api/v3/admin/version/
      headers:
        - key: Authorization
          value: bearer <API Token>
      json: version_current
```

## golang/go
Source: https://github.com/golang/go
```yaml
service:
  golang/go:
    type: url
    url: https://golang.org/dl/
    url_commands:
      - type: regex_submatch
        regex: go([0-9.]+[0-9]+)\.src\.tar\.gz
    web_url: https://go.dev/doc/go{{ version | split:"." | slice:":2" | join:"."  }}
    semantic_versioning: false
    icon: https://go.dev/images/gophers/motorcycle.svg
```

## gohugoio/hugo
Source: https://github.com/gohugoio/hugo
```yaml
service:
gohugoio/hugo:
    type: github
    url: gohugoio/hugo
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/gohugoio/hugo/releases/v{{ version }}
    regex_content: hugo_{{ version }}_Linux-64bit\.deb
    icon: https://raw.githubusercontent.com/gohugoio/hugo/master/docs/static/img/hugo.png
```

## gotify/server
Source: https://github.com/gotify/server
```yaml
service:
  gotify/server:
    type: github
    url: gotify/server
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/gotify/server/releases/v{{ version }}
    icon: https://github.com/gotify/logo/raw/master/gotify-logo.png
    deployed_version:
      url: https://gotify.example.io/version
      json: version
```

## grafana/grafana
Source: https://github.com/grafana/grafana
```yaml
service:
  grafana/grafana:
    type: github
    url: grafana/grafana
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://grafana.com/docs/grafana/latest/release-notes/release-notes-{{ version | split:"." | join:"-"  }}
    icon: https://cdn.worldvectorlogo.com/logos/grafana.svg
    deployed_version:
      url: https://grafana.example.io/login
      regex: v([0-9.]+)\s\([0-9a-z]+\)
```

## grafana/loki
Source: https://github.com/grafana/loki
```yaml
service:
  grafana/loki:
    type: github
    url: grafana/loki
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://grafana.com/docs/loki/latest/release-notes/v{{ version | split:"." | slice:":2" | join:"-"  }}
    icon: https://grafana.com/static/assets/img/blog/loki.png
    deployed_version:
      url: https://loki.example.io/loki/api/v1/status/buildinfo
      json: version
```

## healthchecks/healthchecks
Source: https://github.com/healthchecks/healthchecks
```yaml
service:
  healthchecks/healthchecks:
    type: github
    url: healthchecks/healthchecks
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/healthchecks/healthchecks/releases/tag/{{ version }}
    icon: https://healthchecks.io/static/img/logo-rounded.svg
    deployed_version:
      url: https://healthchecks.example.io/docs/
      regex: Healthchecks v([0-9.]+)
```

## hashicorp/vault
Source: https://github.com/hashicorp/vault
```yaml
service:
  hashicorp/vault:
    type: github
    url: hashicorp/vault
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/hashicorp/vault/releases/v{{ version }}
    icon: https://raw.githubusercontent.com/hashicorp/vault/main/ui/public/vault-logo.svg
    deployed_version:
      url: https://vault.example.io/v1/sys/health
      json: version
```

## hedgedoc/hedgedoc
Source: https://github.com/hedgedoc/hedgedoc
```yaml
service:
  hedgedoc/hedgedoc:
    type: github
    url: hedgedoc/hedgedoc
    regex_version: ^[0-9.]+$
    web_url: https://github.com/hedgedoc/hedgedoc/releases/tag/{{ version }}
    icon: https://raw.githubusercontent.com/hedgedoc/hedgedoc/master/public/icons/android-chrome-512x512.png
    deployed_version:
      url: https://hedgedoc.example.io/config
      regex: window.version = '([0-9.]+)-[0-9a-z]+'
```

## home-assistant/core
Source: https://github.com/home-assistant/core

API_TOKEN from going to /profile and creating a 'Long-Lived Access Token'.
```yaml
service:
  home-assistant/core:
    type: github
    url: home-assistant/core
    web_url: https://www.home-assistant.io/latest-release-notes
    regex_version: ^[0-9.]+$
    icon: https://github.com/home-assistant/core/raw/dev/tests/components/image/logo.png
    deployed_version:
      url: https://home-assistant.example.io/api/config
      headers:
        - key: Authorization
          value: Bearer <API_TOKEN>
```

## louislam/uptime-kuma
Source: https://github.com/louislam/uptime-kuma
```yaml
service:
  louislam/uptime-kuma:
    type: github
    url: louislam/uptime-kuma
    web_url: https://github.com/louislam/uptime-kuma/releases/{{ version }}
    icon: https://raw.githubusercontent.com/louislam/uptime-kuma/master/public/icon.png
```

## mailcow/mailcow-dockerized
Source: https://github.com/mailcow/mailcow-dockerized
```yaml
service:
  mailcow/mailcow-dockerized:
    type: github
    url: mailcow/mailcow-dockerized
    web_url: https://github.com/mailcow/mailcow-dockerized/releases/tag/{{ version }}
    semantic_versioning: false
    regex_version: ^[0-9-]+$
    icon: https://raw.githubusercontent.com/mailcow/mailcow-dockerized/master/data/web/img/cow_mailcow.svg
```

## matomo-org/matomo
Source: https://github.com/matomo-org/matomo
```yaml
service:
  matomo-org/matomo:
    type: github
    url: matomo-org/matomo
    regex_version: ^[0-9.]+$
    web_url: https://github.com/matomo-org/matomo/releases/tag/{{ version }}
    icon: https://raw.githubusercontent.com/matomo-org/matomo/4.x-dev/plugins/CoreHome/images/applogo_732.png
```

## mattermost/mattermost-server
Source: https://github.com/mattermost/mattermost-server
```yaml
service:
  mattermost/mattermost-server:
    type: url
    url: https://mattermost.com/deploy/
    url_commands:
      - type: regex_submatch
        regex: releases\.mattermost\.com\/([^\/]+)\/mattermost-[0-9.]+-linux\-amd64\.tar\.gz
    web_url: https://docs.mattermost.com/install/self-managed-changelog.html
```

## n8n-io/n8n
Source: https://github.com/n8n-io/n8n
```yaml
service:
  n8n-io/n8n:
    type: url
    url: https://github.com/n8n-io/n8n/tags
    url_commands:
      - type: regex_submatch
        regex: n8n\%40([0-9.]+)
    web_url: https://github.com/n8n-io/n8n/blob/master/CHANGELOG.md
    icon: https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-logo.png
```

## netbox-community/netbox
Source: https://github.com/netbox-community/netbox
```yaml
service:
  netbox-community/netbox:
    type: github
    url: netbox-community/netbox
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    regex_version: ^[0-9.]+$
    web_url: https://github.com/netbox-community/netbox/releases/tag/v{{ version }}
    icon: https://github.com/netbox-community/netbox/raw/develop/netbox/project-static/img/netbox_logo.svg
    deployed_version:
      url: https://netbox.lars-lehmann.net/
      regex: '[0-9a-z]+ \(v([0-9.]+)\)'
```

## nextcloud/server
Source: https://github.com/nextcloud/server
```yaml
service:
  nextcloud/server:
    type: github
    url: nextcloud/server
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://nextcloud.com/changelog/#latest{{ version | split:"." | slice:":1" | join:""  }}
    icon: https://github.com/nextcloud/server/raw/master/core/img/favicon.png
    deployed_version:
      url: https://nextcloud.example.io/status.php
      json: versionstring
```

## opencve/opencve
Source: https://github.com/opencve/opencve
```yaml
service:
  opencve/opencve:
    type: github
    url: opencve/opencve
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/opencve/opencve/releases/v{{ version }}
    icon: https://raw.githubusercontent.com/opencve/opencve/master/opencve/static/img/logo_white.png
```

## pterodactyl/panel
Source: https://github.com/pterodactyl/panel
```yaml
service:
  pterodactyl/panel:
    type: github
    url: pterodactyl/panel
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)
    web_url: https://github.com/pterodactyl/panel/releases/v{{ version }}
    icon: https://raw.githubusercontent.com/pterodactyl/panel/develop/public/assets/svgs/pterodactyl.svg
```

## pterodactyl/wings
Source: https://github.com/pterodactyl/wings
```yaml
service:
  pterodactyl/wings:
    type: github
    url: pterodactyl/wings
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)
    web_url: https://github.com/pterodactyl/wings/releases/v{{ version }}
    icon: https://raw.githubusercontent.com/pterodactyl/panel/develop/public/assets/svgs/pterodactyl.svg
```

## prometheus/alertmanager
Source: https://github.com/prometheus/alertmanager
```yaml
service:
  prometheus/alertmanager:
    type: github
    url: prometheus/alertmanager
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/prometheus/alertmanager/blob/main/CHANGELOG.md
    regex_content: alertmanager-{{ version }}\.linux-amd64.tar.gz
    deployed_version:
      url: https://alertmanager.example.io/api/v1/status
      json: version
```

## prometheus/prometheus
Source: https://github.com/prometheus/prometheus
```yaml
service:
  prometheus/prometheus:
    type: github
    url: prometheus/prometheus
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/prometheus/prometheus/blob/main/CHANGELOG.md
    regex_content: prometheus-{{ version }}\.linux-amd64
    deployed_version:
      url: https://prometheus.example.io/api/v1/status/buildinfo
      json: version
```

## rancher/rancher
Source: https://github.com/rancher/rancher
```yaml
service:
  rancher/rancher:
    type: github
    url: rancher/rancher
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/rancher/rancher/releases/v{{ version }}
    icon: https://raw.githubusercontent.com/rancher/docs/master/static/imgs/rancher-logo-cow-blue.svg
```

## requarks/wiki
Source: https://github.com/requarks/wiki
```yaml
service:
  requarks/wiki:
    type: github
    url: requarks/wiki
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    regex_version: ^[0-9.]+$
    web_url: https://github.com/requarks/wiki/releases/tag/v{{ version }}
    icon: https://raw.githubusercontent.com/requarks/wiki/main/client/static/svg/logo-wikijs-full.svg
```

## thanos-io/thanos
Source: https://github.com/thanos-io/thanos
```yaml
service:
  thanos-io/thanos:
    type: github
    url: thanos-io/thanos
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/thanos-io/thanos/blob/main/CHANGELOG.md
    regex_content: thanos-{{ version }}\.linux-amd64
    icon: https://github.com/thanos-io/thanos/blob/main/docs/img/Thanos-logo_fullmedium.png?raw=true
    deployed_version:
      url: https://thanos.example.io/api/v1/status/buildinfo
      json: version
```

## wekan/wekan
```yaml
service:
  wekan/wekan:
    type: github
    url: wekan/wekan
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/wekan/wekan/releases/tag/v{{ version }}
    semantic_versioning: false
    regex_version: ^[0-9.]+$
    icon: https://raw.githubusercontent.com/wekan/wekan/df54863e7243b0b067ec2d30d8352ff1838931c4/meta/icons/wekan-150.svg
```

## wowchemy/wowchemy-hugo-themes
Source: https://github.com/wowchemy/wowchemy-hugo-themes
```yaml
service:
  wowchemy/wowchemy-hugo-themes:
    type: github
    url: wowchemy/wowchemy-hugo-themes
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/wowchemy/wowchemy-hugo-themes/releases/v{{ version }}
```
