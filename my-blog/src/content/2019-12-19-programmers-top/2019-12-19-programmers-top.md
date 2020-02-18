---
layout: post
title: Tower 문제를 DP로 풀기
image: 
author: Jaeyoun
date: 2019-12-19T22:13:47.149Z
tags: 
  - programmers
---

탑(Tower) 문제를 dynamic programming으로 풀기

---

![](./asset-1)

프로그래머스의 레벨 2 스택/큐 문제인 탑 문제를 동적계획법으로 풀어보기

[**코딩테스트 연습 - 탑 | 프로그래머스**  
_수평 직선에 탑 N대를 세웠습니다. 모든 탑의 꼭대기에는 신호를 송/수신하는 장치를 설치했습니다. 발사한 신호는 신호를 보낸 탑보다 높은 탑에서만 수신합니다. 또한, 한 번 수신된 신호는 다른 탑으로 송신되지…_programmers.co.kr](https://programmers.co.kr/learn/courses/30/lessons/42588 "https://programmers.co.kr/learn/courses/30/lessons/42588")[](https://programmers.co.kr/learn/courses/30/lessons/42588)

---

큐/스택으로 푸는 것은 재미없기 때문에, DP를 사용하여 풀어야한다.

DP는 이전 답을 이용해서 다음 답을 구할 수 있을 때 사용할 수 있다.

이 문제에서는 각 탑이 자신의 신호를 수신할 타워를 기록해둠으로써 해결할 수 있다.

예를들어 heights = \[6, 9, 5, 7, 4\] 라면 우리가 목표로 하는 dp배열은 이렇게된다. 하나의 원소는 {idx, height}로 구성된다.

dp = \[{0, 0}, {0, 0}, {2, 9}, {2, 9}, {4, 7}\]

이 dp 배열이 있으면 답은 간단하게 구할 수 있다.

`gist:siisee11/9ae0aea2bbd5bc9bc26afdffd5d15597`


dp 배열을 구하는 것을 중점적으로 보면되는데,

앞의 타워가 클 경우 (16 줄)에 앞의 타워를 dp에 적어주고

그렇지 않을 경우에는 앞 타워들의 정답을 보면서 더 큰 타워가 나오면 그 답을 가져온다.
