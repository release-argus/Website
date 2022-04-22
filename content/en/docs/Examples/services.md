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
services:
  adnanh/webhook:
    type: github
    url: adnanh/webhook
    web_url: https://github.com/adnanh/webhook/releases
    regex_content: webhook-linux-amd64\.tar\.gz
    icon: https://raw.githubusercontent.com/adnanh/webhook/development/docs/logo/logo-128x128.png
```

## ansible/awx-operator
Source: https://github.com/ansible/awx-operator
```yaml
services:
  ansible/awx-operator:
    type: github
    url: ansible/awx-operator
    web_url: https://github.com/ansible/awx-operator/releases
    icon: https://raw.githubusercontent.com/ansible/awx-logos/master/awx/ui/client/assets/logo-login.svg
```

## dani-garcia/vaultwarden
Source: https://github.com/dani-garcia/vaultwarden
```yaml
servcies:
  dani-garcia/vaultwarden:
    type: github
    url: dani-garcia/vaultwarden
    web_url: https://github.com/dani-garcia/vaultwarden/releases
    icon: https://raw.githubusercontent.com/dani-garcia/vaultwarden/main/src/static/images/vaultwarden-icon.png
```

## go-gitea/gitea
Source: https://github.com/go-gitea/gitea:
```yaml
services:
  go-gitea/gitea:
    type: github
    url: go-gitea/gitea
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/go-gitea/gitea/releases
    regex_content: gitea-{{ version }}-linux-amd64
    regex_version: ^[0-9.]+[0-9]$
    icon: https://raw.githubusercontent.com/go-gitea/gitea/main/public/img/logo.png
```

## go-vikunja/api
Source: https://github.com/go-vikunja/api
```yaml
services:
  go-vikunja/api:
    type: url
    url: https://github.com/go-vikunja/api/tags
    url_commands:
      - type: regex_submatch
        regex: \/releases\/tag\/v?([0-9.]+)\"
    web_url: https://github.com/go-vikunja/api/blob/main/CHANGELOG.md
    icon: https://vikunja.io/images/vikunja.png
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
services:
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
services:
gohugoio/hugo:
    type: github
    url: gohugoio/hugo
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/gohugoio/hugo/releases
    regex_content: hugo_{{ version }}_Linux-64bit\.deb
    icon: https://raw.githubusercontent.com/gohugoio/hugo/master/docs/static/img/hugo.png
```

## gotify/server
Source: https://github.com/gotify/server
```yaml
services:
  gotify/server:
    type: github
    url: gotify/server
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/gotify/server/releases
    icon: https://github.com/gotify/logo/raw/master/gotify-logo.png
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
services:
  grafana/loki:
    type: github
    url: grafana/loki
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://grafana.com/docs/loki/latest/release-notes/v{{ version | split:"." | slice:":2" | join:"-"  }}
    icon: https://grafana.com/static/assets/img/blog/loki.png
```

## healthchecks/healthchecks
Source: https://github.com/healthchecks/healthchecks
```yaml
services:
  healthchecks/healthchecks:
    type: github
    url: healthchecks/healthchecks
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/mailcow/mailcow-dockerized/releases/tag/{{ version }}
    icon: https://raw.githubusercontent.com/mailcow/mailcow-dockerized/master/data/web/img/cow_mailcow.svg
    deployed_version:
      url: https://healthchecks.example.io/docs/
      regex: Healthchecks v([0-9.]+)
```

## home-assistant/core
Source: https://github.com/home-assistant/core
```yaml
services:
  home-assistant/core:
    type: github
    url: home-assistant/core
    web_url: https://www.home-assistant.io/latest-release-notes
    regex_version: ^[0-9.]+$
    icon: https://github.com/home-assistant/core/raw/dev/tests/components/image/logo.png
```

## louislam/uptime-kuma
Source: https://github.com/louislam/uptime-kuma
```yaml
services:
  louislam/uptime-kuma:
    type: github
    url: louislam/uptime-kuma
    web_url: https://github.com/louislam/uptime-kuma/releases
    icon: https://raw.githubusercontent.com/louislam/uptime-kuma/master/public/icon.png
```

## mailcow/mailcow-dockerized
Source: https://github.com/mailcow/mailcow-dockerized
```yaml
services:
  mailcow/mailcow-dockerized:
    type: github
    url: mailcow/mailcow-dockerized
    web_url: https://github.com/mailcow/mailcow-dockerized/releases/tag/{{ version }}
    semantic_versioning: false
    regex_version: ^[0-9-]+$
    icon: https://raw.githubusercontent.com/mailcow/mailcow-dockerized/master/data/web/img/cow_mailcow.svg
```

## mattermost/mattermost-server
Source: https://github.com/mattermost/mattermost-server
```yaml
services:
  mattermost/mattermost-server:
    type: url
    url: https://mattermost.com/deploy/
    url_commands:
      - type: regex_submatch
        regex: releases\.mattermost\.com\/([^\/]+)\/mattermost-[0-9.]+-linux\-amd64\.tar\.gz
    web_url: https://docs.mattermost.com/install/self-managed-changelog.html
```

## nextcloud/server
Source: https://github.com/nextcloud/server
```yaml
services:
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
```

## prometheus/prometheus
Source: https://github.com/prometheus/prometheus
```yaml
services:
  prometheus/prometheus:
    type: github
    url: prometheus/prometheus
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/prometheus/prometheus/blob/main/CHANGELOG.md
    regex_content: prometheus-{{ version }}\.linux-amd64
```

## thanos-io/thanos
Source: https://github.com/thanos-io/thanos
```yaml
services:
  thanos-io/thanos:
    type: github
    url: thanos-io/thanos
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/thanos-io/thanos/blob/main/CHANGELOG.md
    regex_content: thanos-{{ version }}\.linux-amd64
    gotify:
      default: {}
    slack:
      default:
        icon_url: https://github.com/thanos-io/thanos/blob/main/docs/img/Thanos-logo_fullmedium.png?raw=true
```

## wowchemy/wowchemy-hugo-themes
Source: https://github.com/wowchemy/wowchemy-hugo-themes
```yaml
services:
  wowchemy/wowchemy-hugo-themes:
    type: github
    url: wowchemy/wowchemy-hugo-themes
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
    web_url: https://github.com/wowchemy/wowchemy-hugo-themes/releases
```
