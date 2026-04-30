## tor-relay
[![Docker Hub](https://img.shields.io/docker/v/chenjia404/tor-relay?sort=semver&label=docker%20hub)](https://hub.docker.com/r/chenjia404/tor-relay)
[![Docker Pulls](https://img.shields.io/docker/pulls/chenjia404/tor-relay?label=pulls)](https://hub.docker.com/r/chenjia404/tor-relay)
[![Image Size](https://img.shields.io/docker/image-size/chenjia404/tor-relay/latest?label=size)](https://hub.docker.com/r/chenjia404/tor-relay)
[![Docker Publish](https://github.com/chenjia404/tor-relay/actions/workflows/monthly-image-rebuild.yml/badge.svg)](https://github.com/chenjia404/tor-relay/actions/workflows/monthly-image-rebuild.yml)

run a tor relay in a container
[中文](./README.zh.md)

middle:
```bash
docker run -d \
	--restart always \
	-v /etc/localtime:/etc/localtime:ro \
	-p 9001:9001 \
		--name tor-relay \
		chenjia404/tor-relay -f /etc/tor/torrc.middle
```

Bridge relay:
```bash
docker run -d \
	--restart always \
	-v /etc/localtime:/etc/localtime:ro \
	-p 9001:9001 \
		--name tor-relay \
		chenjia404/tor-relay -f /etc/tor/torrc.bridge
```

Exit relay:
```bash
	docker run -d \
	--restart always \
	-v /etc/localtime:/etc/localtime:ro \
	-p 9001:9001 \
		--name tor-relay \
		chenjia404/tor-relay -f /etc/tor/torrc.exit
```

Save the key and data of the relay node：
```bash
mkdir -p /home/tor-data
chown -R 100:100 /home/tor-data

docker run -d \
	--restart always \
	-v  /home/tor-data:/var/lib/tor/.tor \
	-v /etc/localtime:/etc/localtime:ro \
	-p 9001:9001 \
		--name tor-relay \
		chenjia404/tor-relay -f /etc/tor/torrc.middle
```


tor-0.4.9.6

### GitHub Actions

This repository includes a GitHub Actions workflow that rebuilds and pushes the Docker image every month.

Add these repository secrets in `Settings > Secrets and variables > Actions`:

| Name | Description |
| --- | --- |
| `DOCKERHUB_USERNAME` | Docker Hub username |
| `DOCKERHUB_TOKEN` | Docker Hub access token |

Workflow file:
`.github/workflows/monthly-image-rebuild.yml`

Triggers:

- On every push to `master`
- Automatically on the 1st day of every month at `03:00 UTC`
- Manually via `workflow_dispatch`

Published tags:

- `latest`
- `YYYYMM`, for example `202604`

Published platforms:

- `linux/amd64`
- `linux/arm64`

### Environment variables

| Name                         | Description                                                                  | Default value |
| ---------------------------- |:----------------------------------------------------------------------------:| -------------:|
| **RELAY_TYPE**               | The type of relay (bridge, middle or exit)                                   | middle        |
| **RELAY_NICKNAME**           | The nickname of your relay                                                   | default |
| **CONTACT_GPG_FINGERPRINT**  | Your GPG ID or fingerprint                                                   | none          |
| **CONTACT_NAME**             | Your name                                                                    | none          |
| **CONTACT_EMAIL**            | Your contact email                                                           | none          |
| **RELAY_BANDWIDTH_RATE**     | Limit how much traffic will be allowed through your relay (must be > 20KB/s) | 10000 KBytes    |
| **RELAY_BANDWIDTH_BURST**    | Allow temporary bursts up to a certain amount                                | 20000 KBytes    |
| **RELAY_PORT**               | Default port used for incoming Tor connections (ORPort)                      | 9001          |
