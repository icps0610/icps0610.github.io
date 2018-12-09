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
`PGUSER=postgres PGPASSWORD=password heroku pg:pull HEROKU_POSTGRESQL_MAGENTA mylocaldb --app sushi`
