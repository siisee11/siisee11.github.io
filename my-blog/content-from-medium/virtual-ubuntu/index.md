---
title: "Virtual Ubuntu 특정 버전으로 커널 버전 바꾸기"
description: "커널 버전을 바꿔보자!"
date: "2019-11-19T14:45:22.454Z"
categories: []
published: true
canonical_link: https://medium.com/@siisee111/virtual-ubuntu-%ED%8A%B9%EC%A0%95-%EB%B2%84%EC%A0%84%EC%9C%BC%EB%A1%9C-%EC%BB%A4%EB%84%90-%EB%B2%84%EC%A0%84-%EB%B0%94%EA%BE%B8%EA%B8%B0-e5555ffc2121
redirect_from:
  - /virtual-ubuntu-특정-버전으로-커널-버전-바꾸기-e5555ffc2121
---

특정 커널 버전을 다운로드 받기

특정 버전의 커널을 사용해야되는 경우가 있다.

-   아래 웹페이지에서 특정 커널 .tar.xz를 다운 받는다. (내 경우에는 4.14.154버전을 다운로드했다.) /usr/src 밑에 다운로드 받는 것이 국룰인듯 하다. 기존의 ubuntu의 커널 소스코드도 /usr/src 밑에 있는 것을 확인할 수 있다. CLI에서 다운로드 방법은 아래와 같다.

[https://mirrors.edge.kernel.org/pub/linux/kernel/](https://mirrors.edge.kernel.org/pub/linux/kernel/) 에서 다운로드

```
$ cd /usr/src
$ wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.14.154.tar.xz
$ tar xvf linux-4.14.154.tar.xz
```

-   다음으로 현재 커널의 configuration파일을 복사해와야한다. (지금 켜져서 돌아가고 있는 커널의 config 파일을 복사하면 일단 켜지긴하니까 그러는 것 같다.)

```
$ cd linux-4.14.154.tar.xz
$ cp /boot/config-$(uname -r) ./.config
```

-   .config 파일을 불러와서 저장해주어야한다. make menuconfig 후에 Load 엔터, Save 엔터, Exit 엔터. 추가적으로 설정해도된다.

```
$ make menuconfig

(error가 뜨고, 혹시 kernel build에 필요한 것들을 깔지 않았을 경우에는 )
sudo apt install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison
```

-   다음은 본격적인 커널 컴파일. 두 가지 방법이 있는 것 같다.   
    첫번째는 make-kpkg를 사용하여 커널 이미지를 만들고 dpkg로 설치.  
    두번째는 보통 컴파일 하듯이 make하는 방법.

```
1. 첫번째 방법
$ sudo make-kpkg --J $(nproc) --initrd --revision=1.0 kernel_image
(revision은 같은 커널 빌드할 때마다 1.0 1.1 이런식으로 바꿔주자.)
$ cd ..
$ dpkg -i <이미지>

2. 두번째 방법
$ make -j $(nproc)
$ sudo make modules_install
$ sudo make install
```

-   마지막으로, 부팅 커널을 설정해주고 재부팅 해주어야한다. ‘/etc/default/grub’ 의 GRUB\_DEFAULT 값을 설정하여 바꿀 수 있는데, 설정값은 ‘/boot/grub/grub.cfg’ 파일을 보거나 ‘update-grub’을 쳐서 확인 할 수 있다. 두번째 방법이 설명하기 편하니 두번째만 살펴보겠다.

```
$ update-grub
``` (결과물 예시)
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.15.0-50-generic
Found initrd image: /boot/initrd.img-4.15.0-50-generic
Found linux image: /boot/vmlinuz-4.15.0-47-generic
Found initrd image: /boot/initrd.img-4.15.0-47-generic

위에서 부터 0, 1, 2, 3... 이렇게 세면 된다.
```

$ vim /etc/default/grub
```
GRUB_DEFAULT="1>숫자" 로 바꿔준다.
```
$ update-grub2
``` 안되면 update-grub ```

$ reboot
```

하면서 뜨는 오류는 구글에 물어보자!
