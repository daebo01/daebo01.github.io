---
title: xen blktap 사용하기
date: 2012-08-30 20:57:00 +0900
categories: []
tags: []
---

```bash
git clone https://github.com/jonludlam/blktap-dkms.git
cd blktap-dkms

make -C /lib/modules/`uname -r`/build M=$PWD
mkdir /lib/modules/`uname -r`/extra
cp blktap.ko /lib/modules/`uname -r`/extra

depmod
modprobe blktap
```
