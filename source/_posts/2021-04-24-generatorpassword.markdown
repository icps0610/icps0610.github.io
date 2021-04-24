---
layout: post
title: "GeneratorPassword"
date: 2021-04-24 13:31:47 +0800
comments: true
categories: golang
---
```

func generatorPassword(n int) string {
    rand.Seed(time.Now().UnixNano())
    table := "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"
    var str string
    table := chrTable()
    for i := 0; i < n; i++ {
        r := rand.Intn(len(table))
        str += string(table[r])
    }
    return str
}
```