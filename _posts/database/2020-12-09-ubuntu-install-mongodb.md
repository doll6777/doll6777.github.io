---
layout: post
title: Ubuntu 18.04에서 MongoDB 설치하기
excerpt: "database"
tags: [programming, database]
permalink: /os/:year/:month/:day/:title/
category : database
---

패키지 업데이트
<pre class="prettyprint">
apt-get update
</pre>

몽고디비 설치
<pre class="prettyprint">
apt-get install mongodb
</pre>

서비스 상태 체크
<pre class="prettyprint">
systemctl status mongodb
</pre>

서비스 재시작
<pre class="prettyprint">
systemctl restart mongodb
</pre>

서비스 중지
<pre class="prettyprint">
systemctl stop mongodb
</pre>

서비스 시작
<pre class="prettyprint">
systemctl start mongodb
</pre>

서버 부팅시 자동 시작
<pre class="prettyprint">
systemctl enable mongodb
</pre>

