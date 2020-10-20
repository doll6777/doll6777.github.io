---
layout: post
title: Git Fetch/Pull/Push Timeout 에러 해결
excerpt: "git"
tags: [programming, git]
permalink: /os/:year/:month/:day/:title/
category : Etc
---

갑자기 특정 레포지토리에서만 타임아웃이 나는 에러가 발생했다.
~/.ssh/config 파일을 생성하고 아래와 같이 입력하면 해결된다.  


```
Host github.com
    Hostname ssh.github.com
    Port 443
```

