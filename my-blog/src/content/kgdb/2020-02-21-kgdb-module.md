---
layout: post
title: Ubuntu18.04 KGDB로 내가 만든 모듈 디버그
image: ubuntu.png
author: Jaeyoun
date: 2020-02-21T16:27:47.149Z
tags: 
  - tools
---

kgdb로 내가 만든 모듈을 디버깅하는 방법이다.

---

이글은 kgdb로 커널 디버깅 환경을 구성한것을 전재로 한다.

포스트 : https://namj.be/kgdb/2020-01-24-kgdb

이 [스택오버플로우](https://stackoverflow.com/questions/6260927/module-debugging-through-kgdb)글을 참고했다.

편의상 kgdb에 대한 다른 포스트들과 마찬가지로 디버거를 돌리는 쪽을 Host, 디버깅을 할 커널을 Guest라고 부르겠다.

---
# Guest
게스트에서 모듈을 컴파일하고 ```insmod module_name```으로 커널에 삽입한다. 실행파일을 하나 만들어 실행시킨다.

```
(Guest)$ vim add-symbol.sh

# 아래 내용을 붙혀넣는다. (당연히 your_module_name을 바꿔주어야한다.)
MODULE_NAME=your_module_name
MODULE_FILE=$(modinfo $MODULE_NAME.ko| awk '/filename/{print $2}')
DIR="/sys/module/${MODULE_NAME}/sections/"
echo add-symbol-file $MODULE_FILE $(cat "$DIR/.text") -s .bss $(cat "$DIR/.bss") -s .data $(cat "$DIR/.data")

(Guest)$ chmod +x add-symbol.sh
(Guest)$ ./add-symbol.sh

add-symbol-file /lib/modules/.../your_module_name.ko 0xffffffffa0110000 -s .bss 0xffffffffa011b948 -s .data 0xffffffffa011b6a0
```
위에서 출력된 결과물을 복사한다.

---

# Host
호스트에서는 gdb에 위에서 복사한 커맨드를 붙혀넣기 해주면 된다.

```
(Host)# gdb vmlinux

(gdb) add-symbol-file /lib/modules/.../your_module_name.ko 0xffffffffa0110000 -s .bss 0xffffffffa011b948 -s .data 0xffffffffa011b6a0
```

> 파일이 없다는 에러가 뜨면 해당위치에 .ko파일을 복사해서 갖다 놓으면 된다.
