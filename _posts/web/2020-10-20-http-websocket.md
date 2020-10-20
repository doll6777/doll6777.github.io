---
layout: post
title: WebSocket에 관하여
excerpt: "web"
tags: [programming, web]
permalink: /web/:year/:month/:day/:title/
category : web
---


### WebSocket
```
클라이언트와 서버간의 full-duplex(양방향), bi-directional(전이중적), persistent connection(지속적인 연결) 의 특징을 갖는 프로토콜이다.
```

2014년 HTML5 버전이 나올 때 함께 등장했다.  

### Http 
#### Polling
클라이언트가 지속적으로 요청을 하고 이벤트를 수신한다. 지속적으로 서버에 요청을 하기 때문에 오버헤드 문제가 발생할 수 있다. 중복적인 패킷전달 (http-header) 문제도 있다.

#### Long Polling
polling과 다른 점은 이벤트를 받기 전까지는 다음 요청을 날리지 않는다. 

#### Http Streaming
서버는 클라이언트로부터 요청을 받으면 응답을 주고 연결을 끊지 않는다. 이벤트가 발생함에 따라 클라이언트로 전송한다.  


```
클라이언트의 요청이 없어도 다음 서버로부터 응답을 받을 수 없을까? 
```


웹 브라우저 환경에서 tcp 환경처럼 ***연결 지향*** 프로토콜이 필요했는데,  이를 해결하기위해 나온 것이 ***WebSocket*** 프로토콜이다.

### TCP
바이너리 데이터만 주고 받을 수 있다.

TCP와 웹소켓의 다른 점은 요청에 대해 switching 및 Handshaking이 이루어진다. 또한 바이너리 뿐만 아니라 텍스트 데이터도 주고 받을 수 있다.  



