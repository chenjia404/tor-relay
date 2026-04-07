## tor-relay
[![Docker Hub](https://img.shields.io/docker/v/chenjia404/tor-relay?sort=semver&label=docker%20hub)](https://hub.docker.com/r/chenjia404/tor-relay)
[![Docker Pulls](https://img.shields.io/docker/pulls/chenjia404/tor-relay?label=pulls)](https://hub.docker.com/r/chenjia404/tor-relay)
[![Image Size](https://img.shields.io/docker/image-size/chenjia404/tor-relay/latest?label=size)](https://hub.docker.com/r/chenjia404/tor-relay)
[![Docker Publish](https://github.com/chenjia404/tor-relay/actions/workflows/monthly-image-rebuild.yml/badge.svg)](https://github.com/chenjia404/tor-relay/actions/workflows/monthly-image-rebuild.yml)

使用docker容器运行tor中继

中继节点:
```bash
docker run -d \
	--restart always \
	-v /etc/localtime:/etc/localtime:ro \
	-p 9001:9001 \
		--name tor-relay \
		chenjia404/tor-relay -f /etc/tor/torrc.middle
```

网桥中继:
```bash
docker run -d \
	--restart always \
	-v /etc/localtime:/etc/localtime:ro \
	-p 9001:9001 \
		--name tor-relay \
		chenjia404/tor-relay -f /etc/tor/torrc.bridge
```

出口节点:
```bash
	docker run -d \
	--restart always \
	-v /etc/localtime:/etc/localtime:ro \
	-p 9001:9001 \
		--name tor-relay \
		chenjia404/tor-relay -f /etc/tor/torrc.exit
```

保存中继节点的key和数据：
```bash
mkdir -p /home/tor-data
chown -R 100:100 /home/tor-data

docker run -d \
	--restart always \
	-v  /home/tor-data:/var/lib/tor/.tor
	-v /etc/localtime:/etc/localtime:ro \
	-p 9001:9001 \
		--name tor-relay \
		chenjia404/tor-relay -f /etc/tor/torrc.middle
```

tor-0.4.7.10

### GitHub Actions

仓库已支持每月自动重新构建并推送 Docker 镜像。

在 GitHub 仓库的 `Settings > Secrets and variables > Actions` 中添加：

| Name | Description |
| --- | --- |
| `DOCKERHUB_USERNAME` | Docker Hub 用户名 |
| `DOCKERHUB_TOKEN` | Docker Hub Access Token |

工作流文件：
`.github/workflows/monthly-image-rebuild.yml`

触发方式：

- 每次 push 到 `master` 时自动执行
- 每月 1 日 `03:00 UTC` 自动执行一次
- 支持在 GitHub Actions 页面手动触发

推送的镜像标签：

- `latest`
- `YYYYMM`，例如 `202604`

支持的镜像架构：

- `linux/amd64`
- `linux/arm64`

### 环境变量

| Name                         | Description                                                                  | 默认值|
| ---------------------------- |:----------------------------------------------------------------------------:| -------------:|
| **RELAY_TYPE**               | 中继类型(bridge, middle or exit)                                              | middle        |
| **RELAY_NICKNAME**           | 节点的昵称                                                                    | default |
| **CONTACT_GPG_FINGERPRINT**  | 你的GPG ID或者指纹                                                            | none          |
| **CONTACT_NAME**             | 你的昵称                                                                      | none          |
| **CONTACT_EMAIL**            | 你的联系邮箱                                                                  | none          |
| **RELAY_BANDWIDTH_RATE**     | 限制允许通过中继的流量 (必须 > 20KB/s)                                          | 10000 KBytes    |
| **RELAY_BANDWIDTH_BURST**    | 允许临时激增最高流量                                                           | 20000 KBytes    |
| **RELAY_PORT**               | 用于传入 Tor 连接的默认端口 (ORPort)                                           | 9001          |
