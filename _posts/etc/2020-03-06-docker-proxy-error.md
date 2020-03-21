---
layout: post
title: Docker 에서 Proxyconnect 관련 에러 나는 경우
excerpt: "git"
tags: [git]
permalink: /etc/:year/:month/:day/:title/
category : etc
---

우분투 16.04 환경에서 Grpc-web을 사용하려고  
docker-compose pull을 하던 중,  

```
ERROR: for common  Get https://registry-1.docker.io/v2/: proxyconnect tcp: tls: first record does not look like a TLS handshake

ERROR: for node-server  Get https://registry-1.docker.io/v2/: proxyconnect tcp: tls: first record does not look like a TLS handshake

ERROR: for commonjs-client  Get https://registry-1.docker.io/v2/: proxyconnect tcp: tls: first record does not look like a TLS handshake

```

이런 에러를 발견하였다.  

```
Hi,

I was also facing same issue behind firewall. Follow below steps:
$ sudo vim /etc/systemd/system/docker.service.d/http_proxy.conf
[Service]
Environment=“HTTP_PROXY=http://username:password@IP:port/”

Don’t use or remove https_prxoy.conf file.

reload and restart your docker
sudo systemctl daemon-reload sudo systemctl restart docker
$ docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:2557*********************************8
Status: Downloaded newer image for hello-world:latest
```

위에 따라 http_proxy.conf 파일을 삭제하고 도커를 리로드/재시작 하니 해결되었다.   

> https://forums.docker.com/t/docker-18-06-behind-proxy-on-centos-7-giving-proxyconnect-tcp-net-http-tls-handshake-timeout/56253/7