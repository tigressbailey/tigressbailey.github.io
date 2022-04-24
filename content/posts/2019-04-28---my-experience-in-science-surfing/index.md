---
title: My Experience for Surfing properly
date: "2019-04-28T12:40:32.169Z"
template: "post"
draft: false
slug: "my-experience-for-surfing-properly"
category: "Devops"
tags:
  - "Devops"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

# Server side

### Install curl

    apt-get update -y && apt-get install curl -y

### Install vvray

    bash &lt;(curl -L -s <https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh>) | tee v2ray_ins.log

### Assets

Once it is installed. The configuration files exist at:

    /etc/v2ray/config.json

    /etc/nginx/conf/conf.d/v2ray.conf

Website:

    /home/wwwroot/levis

Nginx:

    /etc/nginx

Certificates:

    /data/v2ray.key

    /data/v2ray.crt

### Commands

    systemctl start v2ray

    systemctl stop v2ray

    systemctl start nginx

    systemctl restart nginx

    systemctl stop nginx

    systemctl status nginx

### Start services when reboot

    systemctl enable nginx.service

    systemctl disable nginx.service

    systemctl is-enabled nginx.service

    systemctl list-unit-files|grep enabled

### The config

    {
    	"log": {
    		"access": "/var/log/v2ray/access.log",
    		"error": "/var/log/v2ray/error.log",
    		"loglevel": "warning"
    	},
    	"inbounds": \[
        {
        "port":28318,
          "listen": "127.0.0.1",
          "tag": "vmess-in",
          "protocol": "vmess",
          "settings": {
            "clients": [
              {
                "id": "id_1",
                "level": 1,
                "alterId": 233
              },
              {
                "id": "id_2",
                "level": 1,
                "alterId": 233
              },
              {
                "id": "id_3",
                "alterId": 233,
                "level": 1
              },
              {
                "id": "id_4",
                "alterId": 233,
                "level": 1
              }
            ]
          },
          "streamSettings": {
            "network": "ws",
            "wsSettings": {
    	  			"path":"/example_path/"
            }
          }
        }
      ],
      "outbounds": [
        {
          "protocol": "freedom",
          "settings": { },
          "tag": "direct"
        },
        {
          "protocol": "blackhole",
          "settings": { },
          "tag": "blocked"
        }
      ],
      "routing": {
        "domainStrategy": "AsIs",
        "rules": \[
          {
            "type": "field",
            "inboundTag": [
              "vmess-in"
            ],
            "outboundTag": "direct"
          }
        ]
      }
    }

### Add more uuid

    cat /proc/sys/kernel/random/uuid

# Client side

### Homebrew install

-   step 1: Add official tap


    brew tap v2ray/v2ray

-   step 2: Install v2ray-core:


    brew install v2ray-core

### Config path

-   step 1: edit the default config:


    vim /usr/local/etc/v2ray/config.json

-   step 2: run v2ray-core without starting at login.


    brew services start v2ray-core

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
					"path": "/example_path/",  //This path is extremely important
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
