---
layout: post
title: "firt_user_admin"
date: 2017-10-25 23:49:19 +0800
comments: true
categories: ror
---

```
#models/user.rb
class User < ApplicationRecord
	after_create :set_admin

	private

	def set_admin
   if User.count == 1
     User.first.update_attribute(:power,3)
   else
     return true
   end

  end
end
```