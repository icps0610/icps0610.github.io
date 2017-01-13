---
layout: post
title: "ruby_mysql"
date: 2017-01-13 21:24:56 +0800
comments: true
categories: ruby
---

```ruby
require 'active_record'
ActiveRecord::Base.establish_connection({
  adapter: 'mysql2',
  encoding: 'utf8',
  database: 'report',
  username: 'user',
  password: 'passwd'
})
class Event < ActiveRecord::Base
end
```