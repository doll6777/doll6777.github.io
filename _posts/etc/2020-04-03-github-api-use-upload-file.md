---
layout: post
title: Github API 사용하여 파일 업로드하기
excerpt: "git"
tags: [programming, git]
permalink: /os/:year/:month/:day/:title/
category : Etc
---

깃허브 API를 사용하면 파일을 업로드하거나 파일조회, 삭제 등 다양한 작업이 가능하다.  

사용하기 위해서는 Github User 프로필 아이콘을 선택하면 나오는 Settings 화면에서 Github API Token을 발급받아야 한다.  **(Personal Access Token)** 

이 Token은 생성되고 다시 볼 수 없기때문에 생성한 후 바로 자신만 볼수있는 곳에 저장해두어야 한다.  

이때 토큰으로 접근가능한 권한들이 나오는데 자신이 API를 사용하는 용도에따라 체크하면 된다.  

> [Github API Document](https://developer.github.com/v3/repos/contents/)

자세한 사용방법은 다음 사이트에서 확인할 수 있다.  

Enterprise 기준으로 다음과 같이 사용한다.  

```
PUT /repos/:owner/:repo/contents/:path
```

### Example
https://[github 주소]/api/v3/repos/[내 아이디]/[레포지토리 이름]/contents/[생성할 파일 이름]

### curl
<pre class="prettyprint">
curl --request PUT \
  --url [url]] \
  --header 'authorization: token [tokenID]]' \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/json' \
  --header 'token_type: bearer' \
  --data '{\n	"message": "my commit message",\n	"content": [base64로 인코딩된 스트링 내용],\n	"commiter": {\n		"name": "Ran",\n		"email":[my email]\n	}\n}'
</pre>
