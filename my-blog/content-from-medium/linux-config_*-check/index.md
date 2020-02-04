---
title: "Linux CONFIG_* check"
description: "linux ubuntu의 kernel configuration을 체크하는 방법"
date: "2019-11-22T05:36:47.591Z"
categories: []
published: true
canonical_link: https://medium.com/@siisee111/linux-config-check-721baeaee7a0
redirect_from:
  - /linux-config-check-721baeaee7a0
---

linux ubuntu의 kernel configuration을 체크하는 방법

현재 자기가 쓰고 있는 리눅스 커널의 configuration을 확인하는 방법은 다음과 같다.

-   /usr/src/linux-headers-$(uname -r) 이 지금 현재 커널의 header 코드이다.

```
``` $(uname -r)이 현재 커널정보 ```
cd /usr/src/linux-headers-$(uname -r)
```

-   .config 파일에 설정들이 표시되어있다. grep을 이용해서 자신이 확인하고 싶은 configuration 을 확인한다.

```
cat .config | grep 'CLEANCACHE'
CONFIG_CLEANCACHE=y
```
