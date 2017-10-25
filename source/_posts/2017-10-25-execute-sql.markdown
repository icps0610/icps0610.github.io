---
layout: post
title: "execute_sql"
date: 2017-10-25 21:51:05 +0800
comments: true
categories: ruby
---

>> ref https://stackoverflow.com/questions/15408285/rails-3-execute-custom-sql-query-without-a-model
``` ruby
ActiveRecord::Base.establish_connection({adapter: 'mysql2', encoding: 'utf8', database: DB, username: USER,password: PASSWD})

sql = "SELECT * from users"
@result = ActiveRecord::Base.connection.execute(sql);
@result.each do |r| 
   puts r["name"] 
end
```

