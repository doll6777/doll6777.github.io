---
layout: post
title: Git에서 로컬 브랜치 모두 삭제하기
excerpt: "network"
tags: [programming, git]
permalink: /os/:year/:month/:day/:title/
category : Etc
---

<pre class="prettyprint">
git branch | grep -v '^*' | xargs git branch -d
</pre>