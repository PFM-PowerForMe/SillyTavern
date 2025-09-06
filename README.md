# SillyTavern

[![PFM-Upstream-Sync](https://github.com/PFM-PowerForMe/SillyTavern/actions/workflows/fork-sync.yml/badge.svg)](https://github.com/PFM-PowerForMe/SillyTavern/actions/workflows/fork-sync.yml)

## 简介
LLM Frontend for Power Users. 

## 如何部署?

podman example:
```
# /etc/containers/systemd/sillytavern.container

[Unit]
Description=The sillytavern container
Wants=network-online.target
After=network-online.target

[Container]
AutoUpdate=registry
ContainerName=sillytavern
Timezone=local
UserNS=host
Network=deploy
Environment=NODE_ENV=production
Environment=FORCE_COLOR=1
Volume=sillytavern-config:/home/node/app/config
Volume=sillytavern-data:/home/node/app/data
Volume=sillytavern-plugins:/home/node/app/plugins
Volume=sillytavern-extensions:/home/node/app/public/scripts/extensions/third-party
PublishPort=127.0.0.1:6009:8000
Image=ghcr.io/pfm-powerforme/sillytavern:latest

[Service]
Restart=on-failure
RestartSec=30s
StartLimitInterval=30
TimeoutStartSec=900
TimeoutStopSec=70

[Install]
WantedBy=multi-user.target default.target
```