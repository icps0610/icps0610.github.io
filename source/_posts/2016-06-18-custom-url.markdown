---
layout: post
title: "自定網址"
date: 2016-06-18 04:23:26 +0800
comments: true
categories: 
---

``` ruby
class AddUrlToGroups < ActiveRecord::Migration
	def change
		add_column :groups, :url, :string
	end
end
```
 

``` ruby
#controller
def set_group
	@group = Group.find_by_url(params[:id])
end
 
def update
	@group = Group.find(params[:id])
end
```

``` ruby
#model
 
class Group < ActiveRecord::Base
	before_create :generate_url
 
	protected
 
	def generate_url
		require 'digest/md5'
		self.url = Digest::SHA1.hexdigest(self.title + Time.now.localtime.strftime("%m%d%H%M%S").to_s)
	end
end
```
 
``` ruby
#view

<%= link_to group.title, group_path(:controller => "group", :action => "show", :id => group.url) %> 

```