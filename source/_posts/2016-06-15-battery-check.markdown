---
layout: post
title: "battery_check"
date: 2016-06-15 22:04:43 +0800
comments: true
categories: linux
---

`upower -i $(upower -e | grep 'BAT')| grep percentage:| awk '{print$2}'`