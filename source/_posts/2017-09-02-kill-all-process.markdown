---
layout: post
title: "kill_all_process"
date: 2017-09-02 19:07:40 +0800
comments: true
categories: linux
---
pkill -f ngrok

`ps aux | grep  ngrok | awk '{print "kill -9 " $2}'`
