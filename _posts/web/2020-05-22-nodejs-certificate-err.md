---
layout: post
title: NodeJS에서 unable to verify the first certificat 에러 나는 경우
excerpt: "web"
tags: [programming, web]
permalink: /os/:year/:month/:day/:title/
category : web
---

npm install 하니 다음과 같은 에러가 발생했다.  
`unable to verify the first certifiacate`

<pre class="prettyprint">
npm config set registry http://registry.npmjs.org/ --global
</pre>

후에 node_modules를 삭제하고 다시 npm install을 하니 문제가 해결되었다.