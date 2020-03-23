---
layout: post
title: Ubuntu에 Prometheus 설치와 Grafana 연동
excerpt: "git"
tags: [git]
permalink: /etc/:year/:month/:day/:title/
category : etc
---

https://blog.chann.kr/posts/init-prometheus-grafana/


프로메테우스는 시스템 모니터링을 하기 위한 오픈소스다. 기존의 그라파이트와 같은 모니터링 솔루션과는 다르게 Polling 방법을 사용하여 필요한 데이터를 수집한다.  

> https://github.com/prometheus/prometheus

### 패키지 매니저와 systemd 사용하여 실행하기
```
sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
```

```
sudo apt-get update -y
sudo apt-get install -y prometheus prometheus-node-exporter prometheus-pushgateway prometheus-alertmanager
```

```
sudo systemctl start prometheus
sudo systemctl enable prometheus
```


### 도커로 실행하는 법
```
$ docker run --name prometheus -d -p 127.0.0.1:9090:9090 prom/prometheus
```

### 그라파나 설치
데이터 수집을 하였다면 시각적으로 잘 보여주는것이 좋은데, 이것을 가능하게 하는 오픈소스다.  

```
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
curl https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install grafana
```

### 도커로 실행하는 법
```
docker run -d -p 3000:3000 grafana/grafana
```


```
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

### node-exporter 설치
모니터링하려는 시스템에 exporter를 설정한다. 이는 메트릭을 수집하고 프로메테우스가 가져갈 수 있도록 열어주는 것이다.  

```
sudo apt install prometheus-node-exporter
```

```
systemctl reload prometheus.service
```

프로메테우스와 그라파나 둘다 localhost의 해당 포트로 브라우저를 통해 접속할 수 있다. grafana는 id와 pw가 초기 admin/admin 이다.  

데이터소스를 연결할 때 프로메테우스를 설정하고 url을 localhost 대신 실제 아이피를 입력해야 한다.  

필요한 데이터들을 대시보드를 만들고 쿼리를 생성하여 시각화 할 수 있다. 쿼리에 node_로 시작하는 메트릭들을 볼 수 있다.  




