---
layout: post
title: "MySQL_backup"
date: 2016-06-17 00:08:48 +0800
comments: true
categories: ror
---

` mysqldump -u root --password=passwd databasename > db.sql`
`mysql -u root --password=passwd databasename < db.sql`
`echo "select * from column" | mysql -u root -p -A databasename > result.txt`

``` ruby
#backup_mysql.rb
dbs=`mysql -u root --password=passwd -Bse 'show databases'`.split("\n")
dbs.each do |db|
	`mysqldump -u root --password=passwd #{db} > #{db}.sql`
end
```