---
layout: post
title: Linux 서버 정보 콘솔에서 확인하기
excerpt: "OS"
tags: [programming, OS]
permalink: /os/:year/:month/:day/:title/
category : OS
---

- 시리얼 번호 및 모델명 출력
<pre class="prettyprint">
dmidecode | grep Name
dmidecode | grep Serial
</pre>

- CPU 정보 확인
<pre class="prettyprint">
cat /proc/cpuinfo
</pre>

- 메모리 정보 출력
<pre class="prettyprint">
cat /proc/meminfo
</pre>

- 커널 정보 출력
<pre class="prettyprint">
uname -a
</pre>

- 디스크 정보 출력
<pre class="prettyprint">
df -h
</pre>

