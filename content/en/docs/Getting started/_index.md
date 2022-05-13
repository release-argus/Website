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

---
## Installation

---
### Local Binary

#### Download

[Download the latest release](https://github.com/hymenaios-io/Hymenaios/releases) of Hymenaios for your platform.

The Hymenaios server is a single binary named in the style of `hymenaios-VERSION.PLATFORM.ARCH`. We can run this binary and view help on its options by passing the `-help` flag.
```bash
❯ ./hymenaios-0.0.0.linux-amd64 -help
Usage of ./hymenaios-0.0.0.linux-amd64:
  -config.check
        Print the fully-parsed config.
...
```

#### systemd service
Example systemd service file @ `/etc/systemd/system/hymenaios.service`
```
[Unit]
Description=Hymenaios Server
Documentation=https://hymenaios.io/docs
After=network-online.target

[Service]
User=hymenaios
Restart=on-failure
ExecStart=/home/hymenaios/hymenaios \
  -config.file=/home/hymenaios/hymenaios/config.yml

[Install]
WantedBy=multi-user.target
```

Enable and start the service:
```bash
systemctl daemon-reload
systemctl enable hymenaios
systemctl start hymenaios
```
---
### Docker

Hymenaios can be found on the Docker Hub at [hymenaios/hymenaios](https://hub.docker.com/r/hymenaios/hymenaios), GHCR at [ghcr.io/hymenaios-io/hymenaios](https://github.com/hymenaios-io/Hymenaios/pkgs/container/hymenaios) and Quay at [quay.io/hymenaios/hymenaios](https://quay.io/repository/hymenaios/hymenaios).

The tag format for the docker images are:
- `latest` is the latest GitHub release
- `MAJOR.MINOR.PATCH` specific GitHub release
- `master` mirrors the GitHub master branch

Example `docker-compose.yml` that uses the latest release:
```yaml
version: '3.7'

services:
  hymenaios:
    image: hymenaios/hymenaios:latest
    volumes:
      - /path/to/local/config.yml:/etc/hymenaios/config.yml
    ports:
      - 8080:8080
    restart: always
```
deploy with:
```bash
docker-compose up -d
```

---
## Config

To configure the `config.yml`, follow the various guidance [here](/docs/config).

---
## Try it out!

Test the demo [here!](/demo/approvals)
