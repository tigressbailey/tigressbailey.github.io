---
title: Open Certain Ports on Ubuntu
date: "2019-04-02T12:40:32.169Z"
template: "post"
draft: false
slug: "open-certain-ports-on-ubuntu"
category: "Devops"
tags:
  - "Devops"
  - "VPS"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

## Issue the following command to open port 1191 for TCP traffic
```
sudo ufw allow 1191/tcp
```

## Issue the following command to open a range of ports
```
sudo ufw allow 60000:61000/tcp
```

## Issue the following command to stop and start Uncomplicated Firewall (UFW).
```
sudo ufw disable
sudo ufw enable
```

## Issue the following command to list the current firewall policies.
```
sudo iptables -S
sudo iptables -L
```

## Issue the following command to open port 1191 (GPFS™) for inbound TCP traffic from internal subnet 172.31.1.0/24.
```
sudo iptables -A INPUT -p tcp -s 172.31.1.0/24 --dport 1191 -j ACCEPT
```

## Issue the following command to open port 1191 (GPFS) for outbound TCP traffic to internal subnet 172.31.1.0/24.
```
sudo iptables -A OUTPUT -p tcp -d 172.31.1.0/24 --sport 1191 -j ACCEPT
```

## Issue the following command to open port 445 (SMB) for outbound TCP traffic to external subnet 10.11.1.0/24 and only for adapter eth1.
```
sudo iptables -A OUTPUT -o eth1 -p tcp -d 10.11.1.0/24 --sport 445 -j ACCEPT
```

## Issue the following command to open port 445 (SMB) for inbound TCP traffic to a range of CES IPs (10.11.1.5 through 10.11.1.11) and only for adapter eth1.
```
sudo iptables -A INPUT -i eth1 -p tcp -m iprange --dst-range 10.11.1.5-10.11.1.11 --dport 445 -j ACCEPT
```

## Issue the following command to allow an internal network, eth1, to communicate with an external network, eth0.
```
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
```

## Issue the following command to save firewall rule changes to persist across a reboot.
```
sudo iptables-save
```

## Issue the following command to stop and start Uncomplicated Firewall (UFW).
```
service iptables stop
service iptables start
```
