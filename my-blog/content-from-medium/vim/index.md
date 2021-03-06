---
title: "나의 VIM 사용법"
description: "항상 사용하는 vim commands"
date: "2019-12-31T01:39:05.542Z"
categories: []
published: true
canonical_link: https://medium.com/@siisee111/%EB%82%98%EC%9D%98-vim-%EC%82%AC%EC%9A%A9%EB%B2%95-5cf9a0d6416b
redirect_from:
  - /나의-vim-사용법-5cf9a0d6416b
---

항상 사용하는 vim commands

VIM은 오랫동안 존재해온 Text editor이다. 특이한 걸 좋아해서 쓰기 시작했다가 너무 마음에 들어서 계속 쓰고있다.  
오랫동안 쓰이고 현재 많은 IDE에서 VIM을 제공하는 것만 봐도 VIM은 장점이 뚜렷한 에디터임은 틀림없다.

내가 생각하는 VIM의 장점은 아래와 같다.

> 특이하고 빠르고 재밌고 편하다.

이유가 상당히 형편없지만, VIM에 익숙해지면 다른 에디터는 너무 불편하다. 마우스는 생각보다 멀리있다.

VIM의 모든 기능을 설명하기 보다는 내가 주로 사용하는 것들만 나열하겠다.

VIM이 재밌는 이유는 개념만 알고 있으면 응용해서 사용할 수 있다는 것이다.  
아래는 꼭 알아야될 개념들이다.

---

### VIM은 모드가 있다.

```
insert mode
타자치는 대로 화면에 쳐지는 모드

visual mode
커서가는 대로 선택되는 모드

ex mode
그냥 명령어 치는 모드 (esc 누르면 되는 모드)
```

---

### 이동 명령어

```
한 칸 이동
h, j, k, l : 왼, 아래, 위, 오른

단어 이동
w (다음 단어 맨앞 문자로), b (이전 단어 맨뒷 문자로)
e (다음 단어 맨뒷 문자로)

문단 이동
{ (이전 문단)
} (이후 문단)

특정 위치로 이동
gg (파일 맨 앞줄), G (맨 뒷줄)
0 (문장 맨 앞 문자), $ (문장 맨 뒷 문자)
```

---

### 바로 명령어

누르는 순간 수행되는 명령어들이다.

```
삭제
x (커서 있는 문자 삭제), s (커서 있는 문자 삭제하고 insert mode)

삽입
i (커서 앞에서 insert mode 진입), a (커서 뒤에서 insert mode 진입)
p (현재 복사되어 있는 거 붙혀넣기)
o (아랫 줄 삽입), O (윗 줄 삽입)

선택
v (visual 모드로 전환)
ctrl + v (block visual 모드로 전환)

잡기술
* (현재 선택된 단어를 검색, 그 후 n으로 다음으로 이동 가능)
= (indentation 자동 맞춤)
. (이전 명령어 다시 실행)
```

---

### 대기 명령어

누르고나서 범위를 지정해주거나, 범위를 지정해주고 눌러야된다.

```
d : 삭제 or 잘라내기 (dd : 현재 줄 삭제 및 잘라내기)
r : 바꾸기 (r + 바꾸고 싶은 문자)
y : 복사하기 (yy : 현재 줄 복사)
```

---

### VIM의 재밌는 점 : 기본 커맨드들의 조합

위의 커맨드들의 의미를 외우면 이를 조합해서 내가 원하는 명령을 입력할 수 있다.

> 요롷게하면 될까? 하면 된다.

예를 들어서 몇가지를 해보자.

```
현재 커서부터 다음 단어 까지 지우기
 = d (delete) + w (word)

현재 커서부터 현재 줄 끝까지 삭제
 = d (delete) + $ (end of line)

현재 줄 부터 3줄 지우기
 = d (delete) + 3 (number of line) + d (delete)

싹 다 지우기
 = gg (go to start) + d (delete) + G (end of file)

파일 전체의 인덴트 자동 맞춤
 = gg (go to start) + = (auto indentation) + G (end of file)

파일 전체에서 각 줄의 앞 글자 지우기
 = gg (go to start) + ctrl-v (visual block) + w (word) + G (end of file) + d (delete)
```

> 기본을 익히면 응용이되는게 꼭 언어 배우는 것 같다.

---

### 그 외에 많이 사용하는 명령어

```
찾기

/ + 찾을 단어 (정방향 찾기)
? + 찾을 단어 (후방향 찾기)

찾은 후 다음 단어 : n
```
