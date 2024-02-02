---
#categories: ["Examples", "Placeholders"]
#tags: ["test","docs"]
title: "Getting Started"
linkTitle: "Getting Started"
weight: 2
description: >
  Deploying Argus
---

Welcome to Argus! Argus is a software release monitor that can notify and act on new releases that match the filters you set. This guide will show you how to install, configure and monitor your first resource with Argus. You'll download, install and run Argus.

---
## Installation

---
### Local Binary

#### Download

[Download the latest release](https://github.com/release-argus/Argus/releases) of Argus for your platform.

The Argus server is a single binary named in the style of `argus-VERSION.PLATFORM.ARCH`. We can run this binary and view help on its options by passing the `-help` flag.
```bash
❯ ./argus-0.0.0.linux-amd64 -help
Usage of ./argus-0.0.0.linux-amd64:
  -config.check
        Print the fully-parsed config.
...
```

#### systemd service
Example systemd service file @ `/etc/systemd/system/argus.service`
```
[Unit]
Description=Argus Server
Documentation=https://release-argus.io/docs
After=network-online.target

[Service]
User=argus
Restart=on-failure
ExecStart=/opt/argus/argus \
  -config.file=/opt/argus/config.yml

[Install]
WantedBy=multi-user.target
```

Enable and start the service:
```bash
systemctl daemon-reload
systemctl enable argus
systemctl start argus
```
---
### Docker

Argus can be found on the Docker Hub at [releaseargus/argus](https://hub.docker.com/r/releaseargus/argus), GHCR at [ghcr.io/release-argus/argus](https://github.com/release-argus/Argus/pkgs/container/argus) and Quay at [quay.io/argus-io/argus](https://quay.io/repository/argus-io/argus).

The tag format for the docker images are:
- `latest` is the latest GitHub release
- `MAJOR.MINOR.PATCH` specific GitHub release
- `master` mirrors the GitHub master branch

Example `docker-compose.yml` that uses the latest release:
```yaml
version: '3.7'

services:
  argus:
    image: releaseargus/argus:latest
    volumes:
      - /path/to/local/config.yml:/app/config.yml
      - /path/to/storage/:/app/data/ # argus.db
    environment:
      ARGUS_UID: 911 # Optional UID override
      ARGUS_GID: 911 # Optional GID override
    ports:
      - 8080:8080 # <Host_Port:Container_Port>
    healthcheck: # Optional healthcheck
      test: ["CMD", "/healthcheck", "http://localhost:8080/api/v1/healthcheck"]
      interval: 1m
      timeout: 10s
      retries: 3
      start_period: 10s
    restart: always
```
deploy with:
```bash
docker-compose up -d
```

{{< alert title="Note" >}}
If you override the `-config.file` and/or the `-data.database-file` locations, ensure that they are read/writable by the `ARGUS_UID` (default=`911`)
{{< /alert >}}

---
## Config

To configure the `config.yml`, follow the various guidance [here](/docs/config).

---
## Try it out!

Test the demo [here!](/demo/approvals)
