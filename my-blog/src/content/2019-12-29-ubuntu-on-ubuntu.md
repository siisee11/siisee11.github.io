---
layout: post
title: Ubuntu 위에 Ubuntu VM 설치
image: img/ubuntu-logo3.png
author: Jaeyoun
date: 2019-12-29T10:03:47.149Z
tags: 
  - ubuntu
---

공용으로 사용하는 서버의 커널에 toy 모듈 삽입해보다가 서버 한 개 크게 하나 말아먹고, VM을 사용하기로 하였다.

처음에는 더 쉽고 간편한 docker로 하려고했는데, docker로 kernel module개발하는 것은 안될 것이라는 글을 봐서 KVM을 사용하기 위해 설치 해보았다.

아래 커맨드로 virt-manager를 설치할 수 있다. CLI로 vm을 컨트롤하기 어려워서 virt-manager를 사용하는 듯 하다.
```
sudo apt install virt-manager
```
다음으로 가상머신 관련 패키지들을 다운로드 받아야된다.
```
sudo apt install qemu-kvm libvirt-bin bridge-utils ubuntu-vm-builder
```
그 후에 ```sudo virt-manager```를 입력하면 GUI로 된 창이 뜨고 virt-manager를 통해 새로운 VM을 만들어서 사용하면 된다. /var/lib/libvirt/images에 iso파일을 받아두면 virt-manager가 자동으로 인식해서 띄워준다.
```
cd /var/lib/libvirt/images
wget http://mirror.kakao.com/ubuntu-releases/18.04.3/ubuntu-18.04.3-desktop-amd64.iso
```

***

참고 1. 가상머신 설치할 때, 이 디스크에 설치할까요? 선택해도 host의 정보안없어진다.