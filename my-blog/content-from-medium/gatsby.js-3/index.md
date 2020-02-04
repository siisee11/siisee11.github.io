---
title: "Gatsby.js로 블로그 만들기[3]"
description: "gh-pages로 github.io에 올리기"
date: "2019-12-31T15:21:13.770Z"
categories: []
published: true
canonical_link: https://medium.com/@siisee111/gatsby-js%EB%A1%9C-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-3-756bdcfc2874
redirect_from:
  - /gatsby-js로-블로그-만들기-3-756bdcfc2874
---

gh-pages로 github.io에 올리기

![](./asset-1.png)

이전 포스트에서 surge를 통해서 deploy를 하기는 했지만, 도메인 이름이 뭔가 내 것이 아닌 기분이다.

내 것인 듯한 도메인을 무료로 주는 곳이 있으니, 바로 github.

\[1\]에서 username.github.io에 코딩을 하고 있었으니 일단 그렇게 했다는 가정하에 진행한다.

---

가장 먼저 현재 파일들을 master가 아닌 다른 브랜치로 옮겨준다. 추후에 deploy과정에서 master가 덮혀쓰여지기 때문에 개발코드가 날라갈 수 있다.

```
git branch -b blog
git push --set-upstream origin blog
```

이제 gh-pages를 깔아준다.

```
npm install gh-pages --save-dev
```

pakage.json 파일에 다음을 추가한다.

```
{
    "scripts": {
        "deploy": "gatsby build && gh-pages -d public -b master"
    }
}
```

그 후에 아래 커맨드 입력

```
>npm run deploy
```

위 커맨드를 입력하면

public/ 에 스테틱한 파일들이 build되어 생기고

그 파일들이 master 브랜치에 옮겨지고 이를 통해 보여지게 된다.

---

#### 다음포스트

[**Gatsby.js로 블로그 만들기\[4\]**  
_스타일링_medium.com](https://medium.com/@siisee111/gatsby-js%EB%A1%9C-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-4-cebfac8a5cb4 "https://medium.com/@siisee111/gatsby-js%EB%A1%9C-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-4-cebfac8a5cb4")[](https://medium.com/@siisee111/gatsby-js%EB%A1%9C-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-4-cebfac8a5cb4)
