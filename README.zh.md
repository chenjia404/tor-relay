## tor-relay

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

tor-0.4.6.9

### 环境变量

| Name                         | Description                                                                  | 默认值|
| ---------------------------- |:----------------------------------------------------------------------------:| -------------:|
| **RELAY_TYPE**               | 中继类型(bridge, middle or exit)                                              | middle        |
| **RELAY_NICKNAME**           | 节点的昵称                                                                    | hacktheplanet |
| **CONTACT_GPG_FINGERPRINT**  | 你的GPG ID或者指纹                                                            | none          |
| **CONTACT_NAME**             | 你的昵称                                                                      | none          |
| **CONTACT_EMAIL**            | 你的联系邮箱                                                                  | none          |
| **RELAY_BANDWIDTH_RATE**     | 限制允许通过中继的流量 (必须 > 20KB/s)                                          | 1000 KBytes    |
| **RELAY_BANDWIDTH_BURST**    | 允许临时激增最高流量                                                           | 2000 KBytes    |
| **RELAY_PORT**               | 用于传入 Tor 连接的默认端口 (ORPort)                                           | 9001          |