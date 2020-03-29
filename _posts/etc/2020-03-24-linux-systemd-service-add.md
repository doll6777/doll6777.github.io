---
layout: post
title: Linux systemd에 서비스 등록하고 상태 확인하기
excerpt: "linux"
tags: [linux]
permalink: /etc/:year/:month/:day/:title/
category : etc
---

systemd에 사용자가 자주 사용하는 service를 등록하고 관리할 수 있다.  

리눅스에서 서비스를 자동으로 실행되도록 설정하는 것이 편하다면 설정할 수 있다.  

/etc/systemd/system/servicename.service

에 다음과 같은 파일을 등록한다.  

Restart 항목에 on-failure로 등록되어 있는것은 서비스에 문제가 생겼을 때 서비스를 재시작 한다는 뜻이다.  

<pre class="prettyprint">
[Unit]
Description=Service Desceiption

[Service]
Type=simple
ExecStart=/path/to/shellscript.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
</pre>

### 설정 항목
```
Unit
  - Description: 해당 유닛에 대한 설명
  - Requires : 상위 의존성 구성, 포함하는 유닛이 정상적이어야 실행
  - RequiresOverridable : 상위 의존성 구성이며 이것이 실패하더라도 무시하고 유닛을 시작
  ...

Service
  - Type: [simple | forking | oneshot | notify | dbus] 유닛의 타입
  - Environment: 해당 유닛에서 사용할 환경 변수 선언
  - ExecStart: 시작 명령을 정의
  - ExecStop: 중지 명령을 정의

Install
  - WantedBy: 유닛을 등록하기 위한 종속성 검사. 유닛을 등록할 때 등록에 필요한 유닛을 지정
```

이외에도 옵션들이 많으니 궁금하다면 검색하여 더 찾아보면 된다.  

### 등록된 서비스 시작하기
<pre class="prettyprint">
systemctl start servicename
</pre>

### 서비스 상태 체크
<pre class="prettyprint">
systemctl status servicename
</pre>

### 재부팅 후에도 서비스가 실행되도록 등록
<pre class="prettyprint">
systemctl enable servicename
</pre>

### 서비스 중지
<pre class="prettyprint">
ps -ef | grep servicename
kill -9 [pid]
</pre>

### 서비스와 관련된 로그 확인하기
<pre class="prettyprint">
journalctl -u servicename
</pre>
