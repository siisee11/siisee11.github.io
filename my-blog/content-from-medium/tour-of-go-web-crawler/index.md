---
title: "Tour of Go — Web crawler 웹 크롤러"
description: "tour of go 웹크롤러"
date: "2020-01-17T12:30:35.333Z"
categories: []
published: true
canonical_link: https://medium.com/@siisee111/tour-of-go-web-crawler-%EC%9B%B9-%ED%81%AC%EB%A1%A4%EB%9F%AC-fe9eb7f77bb9
redirect_from:
  - /tour-of-go-web-crawler-웹-크롤러-fe9eb7f77bb9
---

tour of go 웹크롤러

![](./asset-1)

Tour of Go 연습문제 1~10은 아래 링크에 정리되어 있다.

[**Go to a Tour of Go**  
_A Tour of Go로 Go와 친해지기_medium.com](https://medium.com/@siisee111/go-to-a-tour-of-go-4b71240db43b "https://medium.com/@siisee111/go-to-a-tour-of-go-4b71240db43b")[](https://medium.com/@siisee111/go-to-a-tour-of-go-4b71240db43b)

Tour of Go의 마지막 연습문제인 웹크롤러는 딱 보면 어렵다.

Go 언어를 이해해야 풀 수 있게 되어있다. Go의 지식을 테스트해보기에 좋은 문제인 것 같다.

쉬운 방법에서 어려운 방법까지 풀어보려고한다.

### 1) channel & defer

채널과 defer를 사용해서 concurrent하게 크롤링을 하는 방법이다.

main함수에서 result라는 이름의 string 채널을 사용해서 Crawl함수와 통신한다. Crawl함수가 재귀적으로 불리면서 수집한 결과를 result채널을 통해 main함수에 넘겨주어 통신하는 것이다.

Crawl함수는 내부적으로 또다른 result 채널을 만들어 재귀적으로 사용한다. url의 개수만큼 result채널을 만든다. main에서 넘겨받은 result채널을 재사용하면 데드락이 발생한다.

```
fatal error: all goroutines are asleep - deadlock!
```

위의 내용대로 구현하면 아래와 같이 구현 할 수 있다.

Ok, but not good

위의 코드를 보면 모든 return 전에 result 채널을 close해주는 것을 볼 수 있다. (만약 그렇게 하지 않으면 deadlock에 빠진다.)

이렇게 해도 프로그램에는 문제 없지만, defer를 사용하면 코드가 더욱 깔끔해진다.

defer는 지연실행인데, 해당 함수가 return 될때까지 코드의 실행을 지연시킨다. 즉, return 되기 직전에 해당 구문을 실행시킨다. defer 를 이용해서 아래와 같이 코드를 변경한다.

defer

하지만 이 방법은 문제를 완벽하게 풀지 못했다. 문제에서는 **중복없이** 크롤링을 하라고 하였는데, 위 코드는 중복을 허용한다.

---

### 2) sync.Mutex

우선 mutex는 한국어판 tour of go에는 설명되어있지 않다. 영어판에서의 링크는 아래 클릭.

[**A Tour of Go**  
_Mutex_tour.golang.org](https://tour.golang.org/concurrency/9 "https://tour.golang.org/concurrency/9")[](https://tour.golang.org/concurrency/9)
