---
layout: post
title: Ubuntu에서 특정 포트를 사용하고 있는 프로세스 종료하기
excerpt: "ubuntu"
tags: [ubuntu]
permalink: /etc/:year/:month/:day/:title/
category : etc
---

### 특정 포트 사용하고 있는 프로세스 종료하기
<pre class="prettyprint">
sudo kill -9 `sudo lsof -t -i:9001`
</pre>

### 특정 포트 사용하고 있는 프로세스 출력하기
<pre class="prettyprint">
sudo netstat -lpn |grep :9001
</pre>

