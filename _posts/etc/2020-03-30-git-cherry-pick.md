---
layout: post
title: Git Cherry Pick 
excerpt: "git"
tags: [programming, git]
permalink: /os/:year/:month/:day/:title/
category : Etc
---

다른 브랜치에 있는 커밋을 선택적으로 내 브랜치에 적용히킬 때 사용하는 명령어다. 이런 상황의 경우에 git rebase를 사용하는것도 한가지의 방법이다. 하지만 rebase는 현재 브랜치에서만 가능하다는 단점이 있다.

<pre class="prettyprint">
git cherry-pick [commit_hash1] [commit_hash2] ...
</pre>

만약 A 브랜치에 있다고 가정하고 B 브랜치의 커밋 여러개를 A에 적용한다고 생각했을 때

<pre class="prettyprint">
git cherry-pick [commit_hash1] [commit_hash2]
</pre>

<pre class="prettyprint">
git cherry-pick [commit_hash1]
git cherry-pick [commit_hash2]
</pre>

### Conflict 발생한 경우
cherry pick할 때 conflict가 발생할 수 있다.  

충돌을 해결하고 add후 --confinue를 붙여주면 다시 진행된다.  
<pre class="prettyprint">
git add <path>
git cherry-pick --confinue
</pre>


### Cherry Pick을 중단하고 싶은경우
<pre class="prettyprint">
git cherry-pick --abort
</pre>
