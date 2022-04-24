---
title: SSL Your Site With Nginx for Localhost ENV
date: "2019-09-09T12:40:32.169Z"
template: "post"
draft: false
slug: "ssl-your-site-with-nginx-for-localhost-env"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

Without further ado, let's get started:

1. Local SSL

   1. Create folder for the key and certificate

   ```
   mkdir -p ~/.localhost-ssl
   ```

   1. Create self signed key

   ```
   sudo openssl genrsa -out ~/.localhost-ssl/localhost.key 2048
   ```

   1. Create self signed key and certificate

   ```
   sudo openssl req -new -x509 -key ~/.localhost-ssl/localhost.key -out ~/.localhost-ssl/localhost.crt -days 3650 -subj /CN=localhost
   ```

   1. Trust the certificate

   ```
   sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain ~/.localhost-ssl/localhost.crt
   ```

2. Add the key and certificate to nginx.conf

   1. Open the nginx.conf

   ```
   vim /usr/local/etc/nginx/nginx.conf
   ```

   1. Modify the server block

   ```
   server {
     listen       3000 ssl;
     ssl                  on;
     ssl_certificate      /Users/username/.localhost-ssl/localhost.crt;
     ssl_certificate_key  /Users/username/.localhost-ssl/localhost.key;
     ssl_ciphers          HIGH:!aNULL:!MD5;
     server_name  localhost;

     #charset koi8-r;

     #access_log  logs/host.access.log  main;

     location / {
         root   html;
         index  index.html index.htm;
         try_files $uri $uri/ /index.html;
     }

     #error_page  404              /404.html;

     # redirect server error pages to the static page /50x.html
     #
     error_page   500 502 503 504  /50x.html;
     location = /50x.html {
         root   html;
     }
   }
   ```
