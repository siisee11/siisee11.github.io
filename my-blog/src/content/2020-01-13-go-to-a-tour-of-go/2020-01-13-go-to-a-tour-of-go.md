---
layout: post
title: Tour of Go 연습문제 답
image: gopher.jpg
author: Jaeyoun
date: 2020-01-13T10:03:47.149Z
tags: 
  - golang
---

A Tour of Go로 Go와 친해지기 ( 연습문제 답 공유)

---

Go 언어를 사용하기 위한 tool을 배웠으니 이제 Go의 문법들을 하나씩 알아가야한다.

Go는 ‘A Tour of Go’ 라는 튜토리얼을 제공한다. 아래는 본문 링크이다.

[**A Tour of Go**  
_Edit description_tour.golang.org](https://tour.golang.org/welcome/1 "https://tour.golang.org/welcome/1")[](https://tour.golang.org/welcome/1)

이 곳에서 쓰인 코드를 다운로드 받으려면

```
$ go get github.com/golang/tour
```

아래는 한국어 번역된 링크이다. 한국어 페이지와 영어페이지가 구성이 다른데, 한국어 페이지를 기준으로 하겠다.

[**A Tour of Go**  
_Go 프로그래밍 언어 투어에 오신 것을 환영합니다. 이 투어는 3개의 섹션으로 되어 있고, 각 섹션의 마지막 부분에는 좀더 완벽한 이해를 돕기 위해 연습문제가 준비되어 있습니다. 지금 Run 버튼을 클릭해보거나…_go-tour-kr.appspot.com](https://go-tour-kr.appspot.com/#1 "https://go-tour-kr.appspot.com/#1")[](https://go-tour-kr.appspot.com/#1)

각 페이지 별로 설명이 잘 되어있기 때문에, 섹션 별로 있는 연습문제에 대한 코드들만 적도록하겠다.

exercise1 — Loop (Newton’s method)

`gist:siisee11/c578ae2d2ab331b2127cb03aa6db2222`

배열을 만들고 값을 대입하는 방법이 핵심이다.

exercise2 — Slice (show picture)

`gist:siisee11/3cc4dfe9529d7517743bf288e8acfae4`

exercise3 — map (Wordcount)

`gist:siisee11/8837c0830b36c9dccbf1b3a8ab9a49c4`

exercise4 — closure (Fibonacci)

`gist:siisee11/e9b2449aa14329263ba9995e9f17fd17`

exercise5— complex (Compley cube root)

`gist:siisee11/8ee92a778906747dd7559e0f22ed34eb`

exercise6 — Error (negative sqrt eror)

`gist:siisee11/b905465eba41b241861bf7e1dd6a92e9`

exercise7 — Http

`gist:siisee11/9af93b0ffc48a117e5491cd7b2836c5a`

exercise8 — Image type

`gist:siisee11/6e3825982b6e4e841c2faf3e9894f628`

exercise9 — Rot13Reader

`gist:siisee11/bf57a6f447d90d07f31268b0fe554f19`

exercise10 — Go

`gist:siisee11/13fff534627471b4aa37aaa3b79a93f9`