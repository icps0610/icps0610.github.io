---
layout: post
title: "line-notify"
date: 2017-06-23 20:45:29 +0800
comments: true
categories: ruby
---
``` ruby
line = "https://notify-api.line.me/api/notify"
to    = ARGV[0]
msg   = ARGV[1]
image = ARGV[2]

case to
when "me"
  token = "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
end
if image and File.exist?(image)
  cmd = "curl -X POST -H 'Authorization: Bearer #{token}' -F 'message=#{msg}' -F 'imageFile=@#{image}' #{line}"
elsif image == nil
  cmd = "curl -X POST -H 'Authorization: Bearer #{token}' -F 'message=#{msg}' #{line}"
else
  cmd = "curl -X POST -H 'Authorization: Bearer #{token}' -F 'message=file cant find' #{line}"
end

system cmd

```