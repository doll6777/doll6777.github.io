---
layout: post
title: 리눅스에서 Anaconda 설치하기
excerpt: "Deep Learning"
tags: [deep learning, programming, dl]
permalink: /deep-learning/:year/:month/:day/:title/
category : 딥러닝
---

> https://www.continuum.io/downloads#linux  
을 통해 각자 자신에게 맞는 버전의 아나콘다 설치 주소를 확인한다.

### wget을 통해 다운로드
<pre class="prettyprint">
wget https://repo.continuum.io/archive/Anaconda2-4.2.0-Linux-x86_64.sh
</pre>

### sh 파일 실행
<pre class="prettyprint">
bash Anaconda2-4.2.0-Linux-x86_64.sh
</pre>

위의 명령어 입력 후 Enter 키를 눌러서 진행한다.  
중간에 라이선스 승인하라는 문구가 나오면 yes를 입력한다.  
> 마지막에 PATH를 추가할 것인지 묻는 창이 나나타는데, yes를 입력해야 한다.

### 설치 완료 확인
<pre class="prettyprint">
conda list
</pre>
를 쳐봤을때, **command not found** 라고 뜨지 않으면 잘 설치된 것이다.

### command not found 뜨는 경우
bash 셸인경우 bashrc, zsh인경우 zshrc 파일에 다음을 추가한다. 
<pre class="prettyprint">
export PATH="/home/username/anaconda2/bin:$PATH"
</pre>

<pre class="prettyprint">
source ~/.bashrc
</pre>








