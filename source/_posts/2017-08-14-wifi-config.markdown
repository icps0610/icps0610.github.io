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

`{% raw %}`echo wpa-psk `wpa_passphrase SSID PASSWORDS | grep -E "\spsk=.*\"?" | cut -f 2 -d "=" ` >> /etc/network/interfaces`{% endraw %}`

``` ruby
#wifi.sh
ssid = ARGV[0]
passwd = ARGV[1]
puts wpa = `wpa_passphrase #{ssid} #{passwd}`
exit if wpa == "Passphrase must be 8..63 characters\n"
psk = wpa.scan(/\tpsk=(.*)/).join

system "echo wpa-ssid #{ssid} >> /etc/network/interfaces"
system "echo wpa-psk #{psk} >> /etc/network/interfaces"

```
