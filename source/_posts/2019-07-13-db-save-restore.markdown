---
layout: post
title: "db_save_restore"
date: 2019-07-13 20:24:04 +0800
comments: true
categories: ror
``` ruby
data = Order.pluck.map{|i|i.map(&:to_s)}
col  = Order.column_names

data = data.map do |table|
    "Order.create" + table.each_with_index.map do |c, i|
        "#{col[i]}: \"#{c}\""
    end.join(",")
end;nil
puts data


```
