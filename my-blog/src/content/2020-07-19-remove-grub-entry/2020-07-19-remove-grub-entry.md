---
layout: post
title: Grub entry 삭제
image: img/ubuntu-logo4.png
author: Jaeyoun
date: 2020-07-19T08:03:47.149Z
tags: 
  - ubuntu
---

Kernel을 마구 바꾸다보면 grub configuration에 너무 많은 Entry가 생긴다.
앞으로 쓸일 없는 kernel entry를 삭제하는 방법이다.

---

예를 들어서 update-grub2를 실행했을 때 
```
(base) root@pmdfc:/lib/modules# update-grub2
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-5.5.0+.old
Found initrd image: /boot/initrd.img-5.5.0+
Found linux image: /boot/vmlinuz-5.3.0-62-generic
Found initrd image: /boot/initrd.img-5.3.0-62-generic
```
위와 같은 결과가 나오고 vmlinuz-5.5.0+.old를 더 이상 사용하지 않기 때문에 이 entry를 지우고 싶다고 가정하자.

먼저, `/boot/` 폴더로 이동해서 `rm vmlinuz-5.5.0+.old`로 파일을 삭제한다.

`/lib/modules/`폴더로 이동해서 해당 버전과 관련된 폴더를 삭제한다.

그 후에 `update-grub2`를 실행하면 사용하지 않는 entry가 깨끗하게 지워진 것을 볼 수 있다.