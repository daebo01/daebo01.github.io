---
title: xen domU - ubuntu 10.04 image 만들기
date: 2012-07-28 15:46:00 +0900
categories: []
tags: []
---

```bash
mkdir /xen/ubuntu
cd /xen/ubuntu

dd if=/dev/zero of=ubuntu.img bs=1M count=8192
```

```bash
fdisk ubuntu.img
```

```
Command (m for help): p

Disk ubuntu.img: 8594 MB, 8594128896 bytes
88 heads, 3 sectors/track, 63581 cylinders, total 16785408 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x482db9e0

       Device Boot      Start         End      Blocks   Id  System
ubuntu.img1            2048    16785407     8391680   83  Linux
n
p
enter
enter
w
```

```bash
losetup /dev/loop0 ubuntu.img \
-o start_sector * sector_size \
--size (end_sector * sector_size) - (start_sector * sector_size)
```

```bash
# xfs 로 하면 pygrub가 안된다
mkfs.ext4 /dev/loop0

mkdir tmp

mount /dev/loop0 tmp
debootstrap --arch i386 lucid tmp http://ftp.daum.net/ubuntu/

chroot tmp

# mem 이 적더라도 필히 pae커널을 사용해야한다.
apt-get install linux-image-generic-pae grub

update-grub

cd /boot/grub/
ln -s menu.lst grub.cfg

nano /etc/apt/sources.list
```

```
deb http://ftp.daum.net/ubuntu lucid main restricted universe multiverse
deb http://ftp.daum.net/ubuntu lucid-updates main restricted universe multiverse
deb http://ftp.daum.net/ubuntu lucid-security main restricted universe multiverse

deb-src http://ftp.daum.net/ubuntu lucid main restricted universe multiverse
deb-src http://ftp.daum.net/ubuntu lucid-updates main restricted universe multiverse
deb-src http://ftp.daum.net/ubuntu lucid-security main restricted universe multiverse
```

그리고 잡세팅..

exit

```
squeeze sources.list
deb http://ftp.daum.net/debian/ squeeze main contrib non-free
deb-src http://ftp.daum.net/debian/ squeeze main contrib non-free
deb http://ftp.daum.net/debian/ squeeze-updates main contrib non-free
deb-src http://ftp.daum.net/debian/ squeeze-updates main contrib non-free
deb http://security.debian.org/ squeeze/updates main contrib non-free
deb-src http://security.debian.org/ squeeze/updates main contrib non-free
```
