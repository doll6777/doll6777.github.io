---
layout: post
title: Python의 가상환경 venv (virtual environment)란?
excerpt: "python"
tags: [python]
permalink: /etc/:year/:month/:day/:title/
category : etc
---

파이썬을 사용할 때 특정 프로젝트에섬산 라이브러리를 사용하고 싶을 때 혹은 배포할 때 필요한 라이브러리만 포함하여  
배고하고 싶을 경우에 사용한다.  

가상환경에서 라이브러리를 추가하면 추가된 라이브러리는 그 가상환경에서만 사용되어진다.  


<pre class="prettyprint">
$ sudo pip install virtualenv
$ virtualenv [envName]
</pre>

envName 은 통상적으로 .venv .env 등이 사용된다.
가상환경을 생성했다면 활성화를 해야 한다. envName/bin/activate 라는 파일을 실행해 가상환경을 실행한다.  

> 주의할 점은, 이 디렉터리 말고 다른곳에서 파이썬을 사용해도 가상환경에서 실행된다. 따라서 다른 프로젝트에서 새로 작업하려면 비활성화를 해야한다.  

비활성호 명령은 다음과 같다.

<pre class="prettyprint">
$ deactivate
</pre>


