---
layout: post
title: "My Experience in Science Surfing"
date: 2019-04-28T22:30:17+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

# Server side

### Install curl
```
apt-get update -y && apt-get install curl -y
```

### Install vvray
```
bash &lt;(curl -s -L <https://git.io/v2ray.sh>)
```
Once it is installed. The configuration files exist at:
```
/etc/v2ray/config.json
/etc/caddy/Caddyfile
```

### The config
```
{
	"log": {
		"access": "/var/log/v2ray/access.log",
		"error": "/var/log/v2ray/error.log",
		"loglevel": "warning"
	},
	"inbounds": \[
		{
			"port": 36505,
			"protocol": "vmess",
			"settings": {
				"clients": [
					{
						"id": "unique-custom-uuid1",
						"level": 1,
						"alterId":233
					},
					{
						"id": "unique-custom-uuid2",
						"level": 1,
						"alterId": 233
					},
					{
						"id": "unique-custom-uuid3",
						"level": 1,
						"alterId": 233
					}
				]
			},
			"streamSettings": {
				"network": "ws"
			},
			"sniffing": {
				"enabled": true,
				"destOverride": [
					"http",
					"tls"
				]
			}
		}
	],
	"outbounds": [
		{
			"protocol": "freedom",
			"settings": {}
		},
		{
			"protocol": "blackhole",
			"settings": {},
			"tag": "blocked"
		},
		{
			"protocol": "freedom",
			"settings": {},
			"tag": "direct"
		},
		{
			"protocol": "mtproto",
			"settings": {},
			"tag": "tg-out"
		}
	],
	"dns": {
		"server": [
			"1.1.1.1",
			"1.0.0.1",
			"8.8.8.8",
			"8.8.4.4",
			"localhost"
		]
	},
	"routing": {
		"domainStrategy": "IPOnDemand",
		"rules": \[
			{
				"type": "field",
				"ip": [
					"0.0.0.0/8",
					"10.0.0.0/8",
					"100.64.0.0/10",
					"127.0.0.0/8",
					"169.254.0.0/16",
					"172.16.0.0/12",
					"192.0.0.0/24",
					"192.0.2.0/24",
					"192.168.0.0/16",
					"198.18.0.0/15",
					"198.51.100.0/24",
					"203.0.113.0/24",
					"::1/128",
					"fc00::/7",
					"fe80::/10"
				],
				"outboundTag": "blocked"
			},
			{
				"type": "field",
				"inboundTag": ["tg-in"],
				"outboundTag": "tg-out"
			}
			,
			{
				"type": "field",
				"domain": [
					"domain:epochtimes.com",
					"domain:epochtimes.com.tw",
					"domain:epochtimes.fr",
					"domain:epochtimes.de",
					"domain:epochtimes.jp",
					"domain:epochtimes.ru",
					"domain:epochtimes.co.il",
					"domain:epochtimes.co.kr",
					"domain:epochtimes-romania.com",
					"domain:erabaru.net",
					"domain:lagranepoca.com",
					"domain:theepochtimes.com",
					"domain:ntdtv.com",
					"domain:ntd.tv",
					"domain:ntdtv-dc.com",
					"domain:ntdtv.com.tw",
					"domain:minghui.org",
					"domain:renminbao.com",
					"domain:dafahao.com",
					"domain:dongtaiwang.com",
					"domain:falundafa.org",
					"domain:wujieliulan.com",
					"domain:ninecommentaries.com",
					"domain:shenyun.com"
				],
				"outboundTag": "blocked"
			}			,
                {
                    "type": "field",
                    "protocol": [
                        "bittorrent"
                    ],
                    "outboundTag": "blocked"
                }
		]
	},
	"transport": {
		"kcpSettings": {
            "uplinkCapacity": 100,
            "downlinkCapacity": 100,
            "congestion": true
        },
		"sockopt": {
			"tcpFastOpen": true
		}
	}
}
```

# Client side

### Homebrew install
- step 1: Add official tap
```
brew tap v2ray/v2ray
```

- step 2: Install v2ray-core:
```
brew install v2ray-core
```

### Config path
- step 1: edit the default config:
```
vim /usr/local/etc/v2ray/config.json
```

- step 2: run v2ray-core without starting at login.
```
brew services start v2ray-core
```

### The config
```
{
	"log": {
		"error": "/Users/username/Documents/v2ray-config/error.log",
		"loglevel": "warning",
		"access": "/Users/username/Documents/v2ray-config/access.log"
	},
	"inbounds": \[
		{
			"port": 1081,
			"listen": "127.0.0.1",
			"tag": "socks-inbound",
			"protocol": "socks",
			"settings": {
				"auth": "noauth",
				"udp": false,
				"ip": "127.0.0.1"
			},
			"sniffing": {
				"enabled": true,
				"destOverride": [ "http", "tls" ]
			}
		}
	],
	"outbounds": \[
		{
			"sendThrough": "0.0.0.0",
			"mux": {
				"enabled": false,
				"concurrency": 8
			},
			"protocol": "vmess",
			"settings": {
				"vnext": \[
					{
						"address": "domainname.cdn.sufix",
						"users": [
							{
								"id": "unique-custom-uuid1",
								"alterId": 233,
								"security": "auto",
								"level": 1
							}
						],
						"port": 443
					}
				]
			},
			"tag": "Seattle",
			"streamSettings": {
				"wsSettings": {
					"path": "/",  //This path is extremely important
					"headers": {}
				},
				"quicSettings": {
					"key": "",
					"security": "none",
					"header": {
						"type": "none"
					}
				},
				"tlsSettings": {
					"allowInsecure": true,
					"alpn": [ "http/1.1" ],
					"serverName": "domainname.cdn.sufix",
					"allowInsecureCiphers": false
				},
				"tcpSettings": {
					"header": {
						"type": "none"
					}
				},
				"security": "tls",
				"network": "ws"
			}
		}
	],
	"routing": {
		"domainStrategy": "IPOnDemand",
		"rules": \[
			{
				"type": "field",
				"outboundTag": "direct",
				"domain": [
					"domain:intranetdomainname1.com",
					"domain:intranetdomainname2.com",
					"domain:intranetdomainname3.com",
					"geosite:cn"
				]
			},
			{
				"type": "field",
				"outboundTag": "direct",
				"ip": [ "geoip:cn", "geoip:private" ]
			}
		]
	},
	"dns": {
		"servers": [ "localhost" ]
	},
	"policy": {
		"levels": {
			"0": {
				"uplinkOnly": 0,
				"downlinkOnly": 0
			}
		},
		"system": {
			"statsInboundUplink": false,
			"statsInboundDownlink": false
		}
	},
	"other": {}
}

```

# SwitchOmega profile
It is in the assets folder

<!--more-->
