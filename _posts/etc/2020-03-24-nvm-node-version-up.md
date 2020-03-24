---
layout: post
title: NVM으로 Node 버전 Downgrade / Upgrade 하기
excerpt: "node"
tags: [node]
permalink: /etc/:year/:month/:day/:title/
category : etc
---

먼저 노드 버전 관리자인 Nvm을 설치한다.  
<pre class="prettyprint">
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
</pre>

설치하고 싶은 노드의 버전을 입력하여 설치한다.  
<pre class="prettyprint">
Usage:
nvm install [version]       
# Download and install a version
nvm use [version]           
# Modify PATH to use version
nvm ls                      
# List versions (installed versions are blue)
</pre>