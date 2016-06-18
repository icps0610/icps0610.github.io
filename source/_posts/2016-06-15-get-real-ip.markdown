---
layout: post
title: "get_real_ip"
date: 2016-06-15 13:36:32 +0800
comments: true
categories: linux
---
`curl -s checkip.dyndns.org | sed -e 's/.*Current IP Address: //' -e 's/<.*$//'`