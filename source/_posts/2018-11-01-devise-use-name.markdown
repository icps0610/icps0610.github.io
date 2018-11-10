---
layout: post
title: "devise_use_name"
date: 2018-11-01 16:30:18 +0800
comments: true
categories: uncategorized
---
```
#config\initalizers\devise.rb
config.authentication_keys = [:name]

#app\model\user.rb
devise :database_authenticatable, :registerable,
    :recoverable, :rememberable, :trackable, :validatable,
    :omniauthable, authentication_keys: [:name]

def email_required?
    false
end
```

