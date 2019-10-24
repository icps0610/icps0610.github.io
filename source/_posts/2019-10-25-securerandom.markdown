---
layout: post
title: "secureRandom"
date: 2019-10-25 02:01:38 +0800
comments: true
categories: golang
---
``` go
func chrTable() string {
    var aZ string
    for i := 65; i <= 90; i++ {
        aZ += string(rune(i))
    }
    for i := 97; i <= 122; i++ {
        aZ += string(rune(i))
    }
    for i := 0; i <= 9; i++ {
        aZ += strconv.Itoa(i)
    }
    return aZ
}

func secureRandom(n int) string {
    rand.Seed(time.Now().UnixNano())
    var str string
    table := chrTable()
    for i := 0; i < n; i++ {
        r := rand.Intn(len(table))
        str += string(table[r])
    }
    return str
}
```