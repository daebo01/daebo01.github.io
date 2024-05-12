---
title: atheros mips cross compile
date: 2012-07-18 01:56:00 +0900
categories: []
tags: []
---

cpu arch에 맞추어 toolchain 을 받습니다.

```bash
wget http://downloads.openwrt.org/kamikaze/7.09/atheros-2.6/OpenWrt-SDK-atheros-2.6-for-Linux-i686.tar.bz2

wget http://downloads.openwrt.org/kamikaze/7.09/atheros-2.6/OpenWrt-SDK-atheros-2.6-for-Linux-x86_64.tar.bz2
```


압축을 풀고 path 를 추가해줍니다.

```bash
export PATH=~/OpenWrt-SDK-atheros-2.6-for-Linux-x86_64/staging_dir_mips/bin/:$PATH
```


```c
// test.c

#include <stdio.h>

int main() {
    printf("hello world");
}
```

```bash
mips-linux-uclibc-gcc test.c -s -o test
```
