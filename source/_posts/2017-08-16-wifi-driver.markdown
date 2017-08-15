---
layout: post
title: "wifi-driver"
date: 2017-08-16 06:35:42 +0800
comments: true
categories: linux
---

TP-LINK Archer T4U driver
>> ref https://askubuntu.com/questions/802205/how-to-install-tp-link-archer-t4u-driver

wget https://github.com/abperiasamy/rtl8812AU_8821AU_linux/archive/master.zip
unzip master.zip
cd rtl8812AU_8821AU_linux-master
make
make install
modprobe rtl8812au