---
layout: post
title: "Legit Scripts for the Servers"
date: 2018-12-29T02:04:24+08:00
author: tigressbailey
sitemap: false
keywords: ""
description: ""
---

# traceroute

`traceroute` is a computer network diagnostic tool for displaying the route (path) and measuring transit delays of packets across an Internet Protocol (IP) network

```
traceroute example.com
```

# vim

https://vimgifs.com/

https://linuxmoz.com/vi-commands-cheat-sheet/

```
u (undo)
control + r (redo)
dd  (delete the line)
/keyword (search word)
n (move forward to next search result)
N (move backward to previous search result)
:q! (discard change and quit)
:wq (save change and quit)
```

# ssh

### access your home directory

```
cd ~
```

### create keygen

```
cd ~/.ssh/
```

```
ssh-keygen -t rsa
```

### view keygen locally

```
less keyname
less keyname.pub
```

```
cat ~/.ssh/my_key.pub
```

### vps routine

Log as root

```
ssh -i ~/.ssh/ my_key root@128.199.166.66
```

### Check server status

```
top
```
htop is better one than just top

#### htop

```
apt-get install htop

htop
```

#### regular update (prefer once a week)

```
apt-get update
```

### add user

```
adduser username
```

#### Grant user access

```
usermod -aG sudo username
```

### switch user

```
su username
```

and

```
exit
```

You will be back to the root.

### sudo user access to visit file

```
cat /var/log/auth.log
```

It will go for `permission denied`

```
sudo !!
```

or

```
sudo cat /var/log/auth.log
```

### ssh with the just created user

```
ssh username@128.199.166.66
```

If the console reports `permission denied (publickey)`, please do following:

1. Open the ssh config

```
vim /etc/ssh/sshd_config
```

2. Modify below config


```
UsePAM yes
IgnoreUserKnownHosts no
PasswordAuthentication no
```

to

```
UsePAM no
IgnoreUserKnownHosts no
PasswordAuthentication yes
```

and save the change

3. restart ssh service

```
service ssh restart
```

Problem resolved!

Let's ssh again and you can login to the server now.

But watch out, login server via the pwd is dangerous. Let introduce a much safer way below.

### ssh server only with private key

It is much better secure comparing to just pwd.

```
cat my_key.pub | ssh username@128.199.166.66 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

What did these lines do?

1. print public key
2. ssh to the server with the username
3. try to create `.ssh` folder
4. copy public key content to the `authorized_keys` file

And it might ask about the pwd.

Juts type in and hit enter.

Now you can login without pwd.

```
ssh username@128.199.166.66
```

### Disabled root auth and password auth for security

1. open the sshd_config

```
sudo vim /etc/ssh/sshd_config
```

2. change below two configs

```
PermitRootLogin yes
PasswordAuthentication yes
```

to

```
PermitRootLogin no
PasswordAuthentication no
```

Save the changes and quit

3. restart the service

```
sudo service ssh restart
```

# Domain

1. buy a domain name. Of course!

2. setup 2 `A Record`. That means:

```
Type: choose 'A Record'
Host: one is '@', another is 'www'
value: 128.199.166.66
```

# Prepare the server - Nginx

1. ssh to the server. Of course!

2. install Nginx

```
sudo apt-get install nginx
```

3. start the service

```
sudo service nginx start
```

Go and verify it via the domain or ip address

4. check the config

```
cat /etc/nginx/sites-available/default
```

or

```
less /etc/nginx/sites-available/default
```