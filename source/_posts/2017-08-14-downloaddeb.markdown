---
layout: post
title: "download_deb"
date: 2017-08-14 12:55:13 +0800
comments: true
categories: linux
---

>> ref https://askubuntu.com/questions/47865/how-do-i-use-apt-get-to-only-download-packages

```
# dldeb.sh
#! /bin/bash
PACKAGE=$1
URI=`apt-cache show $PACKAGE | grep "Filename:" | cut -f 2 -d " "`
wget http://tw.archive.ubuntu.com/ubuntu/$URI
```

```  
#/etc/udev/rules.d/70-persistent-net.rules
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="79:2B:9E:70:67:4F", ATTR{dev_id}=="0x0", ATTR{type}=="1", KERNEL=="wlan*", NAME="wlan0"

SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="C0:E9:F8:12:F6:6A", ATTR{dev_id}=="0x0", ATTR{type}=="1", KERNEL=="wlan*", NAME="wlan1"
```  
