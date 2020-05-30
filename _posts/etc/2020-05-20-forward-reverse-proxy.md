---
layout: post
title: Forward Proxy와 Reverse Proxy
excerpt: "network"
tags: [programming, network]
permalink: /os/:year/:month/:day/:title/
category : Etc
---

### Forward Proxy
Forward Proxy란, 우리가 흔히 생각하는 프록시이다. 우리가 접속하려는 도메인이 http://google.com 일때, 중간에 프록시 정보를 설정하여 프록시를 통해 간접적으로 정보를 가져올 수 있게 된다.  

> 대표적인 포워드 프록시 라이브러리는 Squid이다.

### Reverse Proxy
리버스 프록시는 엔드포인트가 우리가 접속하려는 도메인이 된다. 보통 HA를 위한 Load Balancing이나 보안상의 이유로 쓰인다. 사용자의 요청을 통해 백엔드단의 서버들에게 요청을 분산할 때 유용하게 쓰인다.

> 대표적인 리버스 프록시 라이브러리는 nginx, apache다. 