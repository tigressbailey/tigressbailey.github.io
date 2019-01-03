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