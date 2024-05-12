---
title: HP Smart Array Controller에서 smart 보는 법
date: 2017-04-08 13:41:00 +0900
categories: []
tags: []
---

```bash
smartctl -a -d cciss,N /dev/sdX
```

예) 1번 슬롯과 2번 슬롯이 레이드 1, /dev/sda 일 때

```bash
smartctl -a -d cciss,0 /dev/sda
smartctl -a -d cciss,1 /dev/sda
```
