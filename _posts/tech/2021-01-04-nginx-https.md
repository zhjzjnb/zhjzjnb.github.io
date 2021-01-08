---
layout: post
title: nginx 配置https 反向代理
category: 技术
tags:  nginx 
keywords: nginx https proxy
---



```
自己测试证书生成
openssl genrsa -des3 -out ip.pem 1024
openssl rsa -in ip.pem -out ip.key
openssl req -new -key ip.pem -out ip.csr
openssl x509 -req -days 3650 -in ip.csr  -signkey ip.key -out ip.crt

访问 https://domain/api/ 会代理到proxy_pass并去掉前缀/api 最终路径=>/ /api/login =>/login


server {
    listen 443;
    server_name domain;
    ssl on;
    
    ssl_certificate   /etc/nginx/ssl_key/pem;  #自己证书用crt
    ssl_certificate_key  /etc/nginx/ssl_key/key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        root /var/www/html;
        index index.html index.htm;
    }
    
    location /api/ {
         proxy_pass_header Server;
         proxy_set_header Host $http_host;
         proxy_set_header X-Real-IP $remote_addr;
         proxy_set_header X-Scheme $scheme;
         proxy_pass http://127.0.0.1:3001/;
    }
}

```
