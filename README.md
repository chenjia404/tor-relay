## tor-relay

run a tor relay in a container

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

tor-0.4.2.7

### Environment variables

| Name                         | Description                                                                  | Default value |
| ---------------------------- |:----------------------------------------------------------------------------:| -------------:|
| **RELAY_TYPE**               | The type of relay (bridge, middle or exit)                                   | middle        |
| **RELAY_NICKNAME**           | The nickname of your relay                                                   | hacktheplanet |
| **CONTACT_GPG_FINGERPRINT**  | Your GPG ID or fingerprint                                                   | none          |
| **CONTACT_NAME**             | Your name                                                                    | none          |
| **CONTACT_EMAIL**            | Your contact email                                                           | none          |
| **RELAY_BANDWIDTH_RATE**     | Limit how much traffic will be allowed through your relay (must be > 20KB/s) | 100 KBytes    |
| **RELAY_BANDWIDTH_BURST**    | Allow temporary bursts up to a certain amount                                | 200 KBytes    |
| **RELAY_PORT**               | Default port used for incoming Tor connections (ORPort)                      | 9001          |