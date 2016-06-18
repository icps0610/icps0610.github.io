---
layout: post
title: "line-message"
date: 2016-06-18 03:35:29 +0800
comments: true
categories: ruby
---
``` ruby
#!/usr/bin/env ruby
# encoding: utf-8
loop do
  @content||=""
  printf ">>"
  text = gets
  text  == "@q\n" ? break : @content+=text
end
@content.split("\n").each do |line|
  puts line=line.scan(/\d{2}:\d{2} .* (.*)/)
end
```