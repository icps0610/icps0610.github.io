---
layout: post
title: "portforward"
date: 2016-06-18 03:09:35 +0800
comments: true
categories: ruby
---

> ref http://itszero-blog.tumblr.com/post/947845820/port-forwarding-in-ruby

``` ruby
#!/usr/bin/env ruby
# encoding: utf-8
require 'socket'
listen_port=80
forward_host="127.0.0.1"
forward_port=4000
server = TCPServer.open(listen_port)
loop do
  Thread.start(server.accept) do |client|
    remote = TCPSocket.open(forward_host, forward_port )
    loop do
      r, w, e = IO.select([client, remote], nil, nil, 0)
      (r || []).each do |f|
        if f == remote
          client.write remote.getc.chr
        else
          remote.write client.getc.chr
        end
      end
    end
  end
end   

```