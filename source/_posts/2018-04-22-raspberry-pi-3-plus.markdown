---
layout: post
title: "raspberry_pi_3_plus"
date: 2018-04-22 01:21:33 +0800
comments: true
categories: linux
---

```
# /etc/apt/sources.list
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
deb ftp://ftp.yzu.edu.tw/Linux/raspbian/raspbian/ wheezy main contrib non-free rpi
deb http://raspbian.raspberrypi.org/raspbian/ stretch main contrib non-free rpi
deb-src http://raspbian.raspberrypi.org/raspbian/ stretch main contrib non-free rp
```

### LC_ALL_cannot_change_locale 
```
`export LANGUAGE=en_US.UTF-8`
`export LANG=en_US.UTF-8`
`export LC_ALL=en_US.UTF-8`
`locale-gen en_US.UTF-8`
`dpkg-reconfigure locales`
```

###
```
`apt-get install ruby-dev libsqlite3-dev minicom fuse ntfs-3g`
# fix mount usb => read only
# ==> fuse ntfs-3g

`gem install sinatra`
`gem install bundle`
`bundle install`
eval "$(ssh-agent)";
```

