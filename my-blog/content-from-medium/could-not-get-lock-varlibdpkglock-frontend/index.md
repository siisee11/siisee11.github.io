---
title: "Could not get lock /var/lib/dpkg/lock-frontend 해결"
description: "apt 때매 dpkg lock을 잡았는데 정상 종료 안됐을 때 뜨는 것 같다."
date: "2019-11-18T14:03:37.969Z"
categories: []
published: true
canonical_link: https://medium.com/@siisee111/could-not-get-lock-var-lib-dpkg-lock-frontend-%ED%95%B4%EA%B2%B0-506d09d7f57a
redirect_from:
  - /could-not-get-lock-var-lib-dpkg-lock-frontend-해결-506d09d7f57a
---

apt 때매 dpkg lock을 잡았는데 정상 종료 안됐을 때 뜨는 것 같다.

해결법.

```
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock*

sudo dpkg --configure -a
sudo apt update
```

뭔가 파일을 삭제해주면서 lock이 풀리나보다.
