---
title: Update OpenSSL on Ubuntu
date: "2020-04-23T12:40:32.169Z"
template: "post"
draft: false
slug: "update-openssl-on-ubuntu"
category: "Devops"
tags:
  - "Devops"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

So the context is, lately I read some news about OpenSSL 1.1.1 has a critical vulnerability that can lead TLS 1.3 hand shake crash. Reference: <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967>

It is quite a big deal as it is quite important to my site.

I finally managed to update the OpenSSL today:

```Bash
wget <https://www.openssl.org/source/openssl-1.1.1g.tar.gz>

tar -zxf openssl-1.1.1g.tar.gz

cd openssl-1.1.1g && ./config

make test (It might take 2-3 mins to test and send the PASS message)

mv /usr/bin/openssl ~/tmp

make install

ln -s /usr/local/bin/openssl /usr/bin/openssl

ldconfig

openssl version
```
