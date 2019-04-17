---
layout: post
title: git Invalid username or password 해결법
excerpt: "git"
tags: [git]
permalink: /etc/:year/:month/:day/:title/
category : etc
---


# git에서 Invalid Username/Password 오류 날때 해결법

https://stackoverflow.com/questions/15381198/remove-credentials-from-git

git에서 Windows cmd를 사용해 clone/push를 하는 경우
더이상 아이디/비밀번호를 물어보지 않고
Invalid Password 라고 에러 메세지가 뜨는 경우 해결법

<pre class="prettyprint">
git credential-manager uninstall
</pre>

를 하고, 다시 필요한 git 작업을 하면 새로 아이디/비밀번호를
물어본다.