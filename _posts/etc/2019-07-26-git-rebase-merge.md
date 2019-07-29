---
layout: post
title: Git Rebase Merge (깃 리베이스란?)
excerpt: "git"
tags: [git]
permalink: /etc/:year/:month/:day/:title/
category : etc
---

> https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase%ED%95%98%EA%B8%B0  
https://www.quora.com/What-is-the-difference-between-rebase-and-merge-in-Git

### Rebase란?
머지할 브랜치의 변경된 사항을 패치(Patch)로 만들고 이를 다시 적용시키는 방법이다.
두 브랜치가 나뉘기 전인 공통 커밋으로 이동하고 나서 그 커밋부터 지금 Checkout한 브랜치가 가리키는 커밋까지 diff를 차례로 만들어 어딘가에 임시로 저장해 놓는다. Rebase할 브랜치가 합칠 브랜치가 가리키는 커밋을 가리키게 하고 저장해 놓았던 변경사항을 차례로 적용한다.

### Rebase 하는 방법
```
$ git checkout experiment
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command

```

### Rebase와 Merge의 차이
Rebase든 Merge든 최종 결과물은 같고 커밋 히스토리만 다르다. Rebase는 브랜치의 변경사항을 순서대로 다른 브랜치에 적용하면서 합치고 Merge의 경우는 두 브랜치의 최종결과만을 가지고 합친다.  

Merge는 레파지토리의 히스토리를 모두 저장하지만 여러개의 머지가 있었을때는 특정한 순간에 어떤 일이 있었는지를 쉽게 이해하기가 힘들다.
Rebase는 반면에 히스토리를 다시 쓰기때문에 레포지토리가 더 깔끔하게 보이도록 하여 보기 쉬워지도록 만들어준다.

![image](https://qph.fs.quoracdn.net/main-qimg-5c246cb7872aaed9c1243d3fea96b467.webp)

#### Merge Flow
![image](https://qph.fs.quoracdn.net/main-qimg-27b4c373471ccb22663c3189b051dcc3.webp)

#### Rebase Flow
![image](https://qph.fs.quoracdn.net/main-qimg-531720271c7a9e5ada9047c751c6ab27.webp)



re-base라는 말처럼 베이스브랜치를 바꾼다는 말로 생각할 수 있다.

### Rebase시 주의사항
> 이미 공개 저장소에 Push한 커밋을 Rebase하지 마라!

이 지침만 지키면 Rebase를 할 때 문제 될 게 없다. 리베이스는 기존의 커밋을 그대로 사용하는 것이 아니라 내용은 같지만 다른 커밋을 새로 만든다. 남이 Push한 커밋을 git rebase로 바꿔서 push해버리면 동료가 다시 push했을 때 동료는 다시 Merge해야 하는것이다.

### Merge를 써야 하는가 Rebase를 써야 하는가?
당신의 필요에 따라 달려 있다. 많은 회사들은 모든 변경사항의 히스토리를 봐야 할 필요가 있기 때문에 머지를 의무화하고, 오픈소스 프로젝트나 적은 회사들은 리베이스를 의무화하는데, 플로우를 단순하게 하고 따라가기 쉽게 하기 위해서이다. 각자에게 맞는 방법을 사용하면 된다.