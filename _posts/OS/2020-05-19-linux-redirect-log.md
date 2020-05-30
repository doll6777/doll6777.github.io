---
layout: post
title: Linux에서 콘솔 Log를 저장할때 사용하는 Redirect, Tee에 대하여
excerpt: "OS"
tags: [programming, OS]
permalink: /os/:year/:month/:day/:title/
category : OS
---

#### Redirect와 Pipe의 차이는 무엇일까?
- Redirect : 프로그램의 결과나 출력을 **파일이나 스트림**으로 전달함
- Pipe : 프로세스 혹은 실행된 프로그램의 결과를 **프로그램**으로 넘겨줌

Redirect, Pipe는 IPC(Interprocess Communication) 에 속함

#### 콘솔에서는 보이지 않지만 파일에 저장함
<pre class="prettyprint">
echo "logmessage" &gt; log.txt
</pre>

#### 콘솔에서는 보이지 않지만 이미 있던 파일 뒤에 내용을 이어서 기록함
<pre class="prettyprint">
echo "logmessage" &gt;&gt; tee log.txt
</pre>

#### 콘솔에서도 보이고 파일에 저장함
<pre class="prettyprint">
echo "logmessage" | tee log.txt
</pre>

#### 콘솔에서도 보이고 이미 있던 파일 뒤에 내용을 이어서 기록함
<pre class="prettyprint">
echo "logmessage" | tee -a log.txt
</pre>

#### tee의 기본적인 사용법
<pre class="prettyprint">
tee [ -a ] [ -i ] [ File ... ]
</pre>

- File : 출력을 저장하고 싶은 파일들을 입력해준다.
- -a : 파일이 존재하는 경우 파일을 지우지 않고 내용을 추가한다.
- -i : 인터럽트를 무시한다. 

#### stderr를 stdout으로 출력하가
<pre class="prettyprint">
foo.sh 2&gt;&amp;1 output.txt
</pre>

#### /dev/null 2>&1 의 의미
표준 출력을 /dev/null로 redirection하라는 의미로, **표준출력을 버리라는 의미이다.**

> /dev/null 파일은 항상 비어있으며, /dev/null에 전송된 데이터는 버려진다. 따라서 출력이 필요없는 경우는 /dev/null로 출력을 지정한다.

#### redirect 사용시 표준출력과 표준에러를 분리해서 파일로 저장하고 싶은 경우
- 0 : 표준입력
- 1 : 표준출력
- 2 : 표준에러

<pre class="prettyprint">
foo.sh 1&gt;output.txt 2&gt;error.txt
</pre>
