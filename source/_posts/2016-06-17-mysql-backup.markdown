---
layout: post
title: "MySQL_backup"
date: 2016-06-17 00:08:48 +0800
comments: true
categories: ror
---

`mysqldump -u root -p databasename > db.sql`
`mysql -u root -p databasename > db.sql`
`echo "select * from column" | mysql -u root -p -A databasename > result.txt`