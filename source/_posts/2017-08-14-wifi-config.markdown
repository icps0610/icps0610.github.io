---
layout: post
title: "wifi-config"
date: 2017-08-14 14:44:38 +0800
comments: true
categories: linux
---
```
#/etc/network/interfaces
auto wlan0
iface wlan0 inet static
  address 192.168.0.201
  netmask 255.255.255.0
  gateway 192.168.0.1
  wpa-driver wext
  wpa-ssid SSID
```

`echo wpa-psk `wpa_passphrase SSID PASSWORDS | grep -E "\spsk=.*\"?" | cut -f 2 -d "=" ` >> /etc/network/interfaces`