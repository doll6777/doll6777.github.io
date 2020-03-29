---
layout: post
title: Ubuntu에서 Thread 정보 가져오기
excerpt: "ubuntu"
tags: [ubuntu]
permalink: /etc/:year/:month/:day/:title/
category : etc
---

```
ps -eLf | grep [Processname]
```

위 명령어를 실행하면 

```
UID PID PPID LWP C NLWP STIME TTY TIME CMD
```

의 순서로 스레드의 리스트를 볼 수 있다.  

이때 LWP는 Light Weight Process의 약자로, Thread의 아이디를 의미한다.  

그렇다면 해당 스레드가 호출하는 시스템 함수를 확인하는 방법은?

```
strace -p [LWP]
```

로 어떤 시스템 함수를 호출하고 있는지를 확인할 수 있다.  