---
layout: post
title: "two wods regex"
date: 2017-08-21 04:15:41 +0800
comments: true
categories: uncategorized
---
>> ref https://stackoverflow.com/questions/10678823/regex-format-for-two-strings
array = ["happy","good"]
p array.map{|s|s.scan(/\b(hppy|good)\b/).join}