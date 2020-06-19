---
layout: post
title: Android ADB로 USB 없이 원격으로 연결하기
excerpt: "android"
tags: [android]
permalink: /etc/:year/:month/:day/:title/
category : android
---

ADB를 Wifi를 사용하여 원격으로 연결하는 방법이다.

1. 디바이스와 USB 연결

2. ADB와 tcp로 접속할 것을 세팅
<pre class="prettyprint">
adb tcpip 5555
</pre>

3. 디바이스에 접속
<pre class="prettyprint">
adb connect {ANDROID_IP}:5555
</pre>

4. ADB SHELL 접속
<pre class="prettyprint">
adb shell
</pre>
