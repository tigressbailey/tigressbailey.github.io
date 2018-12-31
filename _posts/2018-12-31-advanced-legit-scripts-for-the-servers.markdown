---
layout: post
title: "Advanced Legit Scripts for the Servers"
date: 2018-12-31T01:08:37+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

# Update server regularly via apt
```
apt update
apt list --upgradable
```

# Install the specific version nodejs on server
```
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
```
and
```
sudo apt install nodejs
```

# Check server open ports from your local laptop
```
nmap SERVER_IP
```
Advanced info for the connection info of all the ports
```
nmap -sV SERVER_IP
```

# Control connection using iptables(High dangerous and not recommend to new users)
```
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
- `-A append rule`
- `-p protocol (tcp, icmp)`
- `--dport destination port`
- `-j jump (DROP, REJECT, ACCEPT, LOG)`

For example:
1. Create an iptable rule to block all outgoing HTTP connections
```
iptables -A OUTPUT -p tcp --dport 80 -j REJECT
```

2. Create an iptable rule to only allow icmp connections on port 892 from the IP address 192.0.0.1
```
iptables -A INPUT -s 192.0.0.1 -p icmp --dport 892 -j ACCEPT
```

# Control connection using uncomplicated firewall
```
sudo ufw allow ssh
sudo ufw enable
```
And block all outgoing HTTP connections
```
ufw reject out http
```