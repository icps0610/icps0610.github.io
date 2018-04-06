---
layout: post
title: "rtl8192eu-linux-drives"
date: 2018-03-20 23:38:25 +0800
comments: true
categories: linux
---
`https://github.com/lord2y/rtl8192eu-arm-linux-driver`

```
apt-get install git raspberrypi-kernel-headers build-essential dkms
dkms add .
dkms install rtl8192eu/1.0

dkms status

dkms uninstall rtl8192eu/1.0  
dkms remove rtl8192eu/1.0 --all  

`https://icps0610.github.io/blog/2018/03/21/wifi-ap/`
```