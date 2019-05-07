---
layout: post
title: "postgresql"
date: 2018-12-09 17:55:32 +0800
comments: true
categories: ror
---

### install 
`apt-get install postgresql`

### change passwd
`sudo -u postgres psql postgres`  
`\password postgres`  

### gem
`gem install pg`

### database.yml
```
default: &default
  adapter: postgresql
  encoding: unicode
  username: postgres
  password: password
  host: localhost
  pool: 5

development:
  <<: *default
  database: database_development
```

### heroku
`su postgres`
`PGUSER=postgres PGPASSWORD=password heroku pg:pull HEROKU_DATABASE_NAME LOCALDB --app sushi`

### rails
`rails new myapp --database=postgresql`


### wsl
>>ref https://github.com/Microsoft/WSL/issues/3863
`data_sync_retry = on`
>>ref https://github.com/Microsoft/WSL/issues/1761
`export RUNLEVEL=1`