---
title: Advanced Legit Scripts for the Servers
date: "2018-12-31T12:40:32.169Z"
template: "post"
draft: false
slug: "advanced-legit-scripts-for-the-servers"
category: "Devops"
tags:
  - "Devops"
  - "VPS"
description: "Advanced Legit Scripts for the Servers."
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
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

# Check server open ports from the local laptop
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

# Update system dependencies automatically
```
sudo apt install unattended-upgrades
```
Check the config is right
```
cat /etc/apt/apt.conf.d/20auto-upgrades
```
It should be
```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```
Only update security packages because the software package may change a lot and break something
```
sudo vi /etc/apt/apt.conf.d/50unattended-upgrades
```
Comment the `"${distro_id}:${distro_codename}";` line and the file would be:
```
Unattended-Upgrade::Allowed-Origins {
//      "${distro_id}:${distro_codename}";
        "${distro_id}:${distro_codename}-security";
        // Extended Security Maintenance; doesn't necessarily exist for
        // every release and this system may not have it installed, but if
        // available, the policy for updates is such that unattended-upgrades
        // should also install from here by default.
        "${distro_id}ESM:${distro_codename}";
//      "${distro_id}:${distro_codename}-updates";
//      "${distro_id}:${distro_codename}-proposed";
//      "${distro_id}:${distro_codename}-backports";
};

// List of packages to not update (regexp are supported)
Unattended-Upgrade::Package-Blacklist {
//      "vim";
//      "libc6";
//      "libc6-dev";
//      "libc6-i686";
};
```

# Install Fail2ban and check the ban log
1. Install it
```
sudo apt install fail2ban
```
2. Copy jail file
```
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```
3. Edit your own strategy
```
sudo vi /etc/fail2ban/jail.local
```
> If you misconfigure fail2ban, you can lock yourself out of your server!
4. Check the ban history
```
sudo tail -f /var/log/fail2ban.log
```

5. Find and Grep
- Find: filter file names
```
find /directory -name filename.txt
```
  - `-name` can be replaced with `type`, `empty`, `executable` and `writable`
  - For example, Find all empty files in /etc
  ```
  find /etc -type f -empty
  ```
  - Find all directories with the word log
  ```
  find / -type d -name log
  ```
- Grep: filter file contents
```
grep -i ‘jem’ /var/www
```
  - Search in gzip file
  ```
  zgrep FILE
  ```
- Find running node processes
```
ps aux | grep node
```

# Redirection operators
```
|
read from stdout
>
write stdout to file
>>
append stdout to file
<
read from stdin
2>
read from stderr
```
For example: Read from bar to foo and write in baz
```
foo < bar > baz
```

#Shell

### Display current shell
```
echo $0
```

### chmod

#### Andybody can read, write, execute.
```
chmod 777 filename
chmod -R 777 dir
```
#### Owner&Group can read, write, execute. Everyone else can read, execute.
```
chmod 775 filename
chmod -R 775 dir
```
#### Owner & Group can read, write, execute. Everyone else can read.
```
chmod 774 filename
chmod -R 774 dir
```
#### Owner can read, write, execute. Everyone else can read, execute.
```
chmod 755 filename
chmod -R 755 dir
```
#### Owner can read, write, execute. No one else has any rights.
```
chmod 700 filename
chmod -R 700 dir
```
#### Everyone can read, write.
```
chmod 666 filename
chmod -R 666 dir
```

#### Owner & Group can read, write. Everyone else can read.
```
chmod 664 filename
chmod -R 664 dir
```

#### Owner can read, write. Everyone else can read.
```
chmod 644 filename
chmod -R 644 dir
```

# HTTPS

## Use certbot

### Add certbot repo
```
sudo add-apt-repository ppa:certbot/certbot
```

### Pull in new repository information
```
sudo apt update
```

### Install Certbot
```
sudo apt install python-certbot-nginx
```

### Use certbot to get certificate
```
sudo certbot --nginx
```

### Test auto renew
```
sudo certbot renew --dry-run
```

# Cron - run task periodically

### open crontab for editing
```
sudo crontab -e
```

In this file add
```
00 12 * * 1 certbot renew
```
That means renew certificate every week at 12PN on Monday

[Know the cron job mean](https://crontab.guru/#5_4_*_*_5)

# Gzip

Open the nginx config file
```
sudo vi /etc/nginx/nginx.conf
```
Add
```
gzip     on;
```

Refer this for further options
```
http://nginx.org/en/docs/http/ngx_http_gzip_module.html
```

# Expires headers

Nginx will provide `Etag` originally.
That means when the file doesn't change, the server will only send back a Etag.
And the client request status will be 304 - Not modified.

But it bring a request from the client browser eventually.
Here we comes the expires headers!

## Set expires headers

Open site file
```
sudo vi /etc/nginx/sites-available/default
```
Add settings for expiring static folder's assets in 30 days
```
location /static/ {
  expires 30d;
  proxy_pass http://127.0.0.1:3001/static/;
}
```
Relod Nginx service and verify the result.
The static assets will be loaded from disk cache(or memory cache) and the request status is 200.
The `cache-control` and `expires` will match 30 days.

## Cache

Open site file
```
sudo vi /etc/nginx/sites-available/default
```
Add settings for cache path etc..
```
proxy_cache_path /tmp/nginx levels=1:2 keys_zone=slowfile_cache:10m inactive=60m use_temp_path=off;

proxy_cache_key "$request_uri";
```
Add cache folder
```
location /slowfile {
  proxy_cache_valid 1m;
  proxy_ignore_headers Cache-Control;
  add_header X-Proxy-Cache $upstream_cache_status;
  proxy_cache slowfile_cache;
  proxy_pass http://127.0.0.1:3001/slowfile;
}
```
Verify it in the browser and see the response header's `X-Proxy-Cache` will from `MISS` to `HIT`

## Websockets

Open site file
```
sudo vi /etc/nginx/sites-available/default
```
Add websocket to notify the upgrade in the `location  / {}` section
```
location / {
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";

  proxy_pass http://127.0.0.1:3001;
}
```

## HTTP2
Open site file
```
sudo vi /etc/nginx/sites-available/default
```
Modify the certbot listen line from `listen 443 ssl;` to below
```
listen 443 http2 ssl; managed by certbot
```

## Redirect
Open site file
```
sudo vi /etc/nginx/sites-available/default
```
Add a permanent redirect(Will be cache by the search engine)
```
location /help {
  return 301 https://developer.mozilaa.org/en-US/;
}
```