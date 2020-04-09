---
layout: post
title: 채점 Automation
image: honey.jpg
author: Jaeyoun
date: 2020-04-9T14:03:47.149Z
tags: 
  - tools
---

채점 자동화를 위한 쉘스크립트

---

# Shell script
Shell Script bash 쉘에 직접 치던 명령어들을 한꺼번에 실행시키는 역할을 한다.
Shell Script는 조금 익혀두면 반복되는 작업들을 빠르게 처리할 수 있는 장점이있다.
리눅스에 익숙하다면 나름 조금의 투자로 빠르게 익힐 수 있다.

가령, 모든 학생들에 대해 directory를 만들어주는 작업을 할 때,
Shell script를 사용하지 않으면, 일일이 
```
$ mkdir 123
$ mkdir 124
```
를 해주어야한다.

하지만 학생들의 학번이 주어진 파일이 있다면, 그냥 Shell script하나 작성해서 돌리면된다.
한번 뿐만 아니라 다음학기 다다음학기에도 사용할 수 있다.

```
#!/bin/bash

while read line
do
        mkdir $line

done < student.txt
```

> Shell script의 문법이나 작성법은 필요한 거 있을 때마다 찾아가면서 작성하면 된다.


# 필요조건 (실행 커맨드)
자동화를 하려면 필수적으로 모든 코드들이 같은 커맨드를 통해 동작되어야한다.

예를 들어 c로 작성하는 과제였다면, 공통적으로
```gcc -o a.out *.cpp```커맨드를 통해서 컴파일이 가능하고
```./a.out```을 통해 실행시킬 수 있다.

만약 여러 언어들을 허용해야한다면, Makefile을 같이 제출하여 ```Make run```과 같은 커맨드로 프로그램이 실행되도록 해야한다.

# 필요조건 (디렉토리 구조화)
모든 제출 파일들이 같은 구조로 저장되어 있어야한다.
```
+-- submitted
|   +-- student1
|         +-- src.cpp
|   +-- student2
|         +-- src.cpp
```

# 자동 채점 프로그램 작성
위의 구조에서 submitted 폴더 아래에서 Shell Script를 작성한다고 가정하자.

```
submitted$ vim grade.sh
```

가장 첫번째로는 디렉토리를 하나하나 들어가는 것이다.

```
for dir in **
do
  if [ -d $dir ]; then
    echo $dir
    cd $dir
done
```
디렉토리에 대해서 dir가 존재한다면 그 디렉토리로 들어가는 코드이다.

그 후에 컴파일하고 실행하고 결과를 out.txt로 받는다.
$> 는 표준 에러와 표준 출력을 redirection한다.

```
make
make run < input.txt > out.txt
```

그 후에 out과 ans를 diff하여 diff 결과가 빈 값인지 아닌지 검사한다.

```
  diff -bwi ans.txt out.txt &> diff.txt

  size=$(stat -c%s diff.txt)
  # identical
  if [ $size -eq 0 ]; then
    echo "correct"
  else
    echo "incorret"
  fi
```

위의 작업을 여러 인풋 파일에 대하여 진행하려면 큰 for문으로 감싸주면 된다.

```
for i in 1 2 3
do
  echo "test input$i"
done
```
