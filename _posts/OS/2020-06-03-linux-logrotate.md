---
layout: post
title: Linux LogRotate 사용하기
excerpt: "OS"
tags: [programming, OS]
permalink: /os/:year/:month/:day/:title/
category : OS
---


### 로그 로테이션이란?
로그의 비대화를 막기 위한 방법이다. 서비스를 운영할 때 로그가 많아지는데 이것을 계속  
이어서 저장하게 하는것은 시스템의 용량을 많이 잡아먹는다. 따라서 로그를 나눠서 저장하고  
일정 시간이 지나면 삭제하는 방법이 필요하다. 이런 작업을 로테이션이라고 한다.  

```logrotate```는 리눅스 시스템에는 기본적으로 설치되어 있다.  


### man logroate 확인
```
DESCRIPTION
       logrotate is designed to ease  administration  of  systems
       that generate large numbers of log files.  It allows auto‐
       matic rotation, compression, removal, and mailing  of  log
       files.   Each  log  file  may  be  handled  daily, weekly,
       monthly, or when it grows too large.

       Normally, logrotate is run as a daily cron job.   It  will
       not modify a log more than once in one day unless the cri‐
       terion for that log is based on the log's size and  logro‐
       tate  is  being run more than once each day, or unless the
       -f or --force option is used.

       Any number of config files may be  given  on  the  command
       line. Later config files may override the options given in
       earlier files, so the order in which the logrotate  config
       files  are listed is important.  Normally, a single config
       file which includes  any  other  config  files  which  are
       needed  should be used.  See below for more information on
       how to use the include directive to accomplish this.  If a
       directory is given on the command line, every file in that
       directory is used as a config file.

       If no command line arguments  are  given,  logrotate  will
       print  version  and  copyright  information,  along with a
       short usage summary.  If any errors occur  while  rotating
       logs, logrotate will exit with non-zero status.
```

### 로그 로테이션 설정하기
<pre class="prettyprint">
/etc/lograte.d
</pre>

해당 경로에 {projectName} 이름의 파일을 만들고, 설정파일을 넣는다.
설정 파일의 예제는 구글링링이나, man logrotate를 치면 잘 나와있다.

### 설정파일 Example
<pre class="prettyprint">
       /var/log/messages {
           rotate 5
           weekly
           postrotate
               /usr/bin/killall -HUP syslogd
           endscript
       }
</pre>


### 로그 로테이션 실행
<pre class="prettyprint">
$ /usr/sbin/logrotate -f /etc/logrotate.d/{projectName}
</pre>



