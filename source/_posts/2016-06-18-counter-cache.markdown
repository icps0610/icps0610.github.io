---
layout: post
title: "Counter-cache"
date: 2016-06-18 04:21:16 +0800
comments: true
categories: ror
---

`$rails g migration add_votes_to_store`
 
``` ruby
def self.up
	add_column :stores, :votes_count, :integer, :default => 0
end
```
  
``` ruby
#model vote
 
class Vote < ActiveRecord::Base
	belongs_to :store, :counter_cache => true
end
```
 
 
``` ruby
#view
<%= store.votes_count %>
```