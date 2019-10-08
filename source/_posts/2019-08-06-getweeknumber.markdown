---
layout: post
title: "getWeekNumber"
date: 2019-08-06 00:49:19 +0800
comments: true
categories: golang
---
``` go
func leapYear(y int) bool {
    if (y%4 == 0 && y%100 != 0) || (y%400 == 0 && y%3200 != 0) {
        return true
    }
    return false
}
func getWeek(y, m, d int) int {
    monthTable := []int{6, 2, 2, 5, 0, 3, 5, 1, 4, 6, 2, 4}
    centuryTable := []int{0, 5, 3, 1}
    monthValue := monthTable[m-1]
    yearValue := y % 100
    yearOver := yearValue / 4
    centuryValue := centuryTable[(y/100)%4]
    w := d + monthValue + yearValue + yearOver + centuryValue
    if leapYear(y) {
        w -= 1
    }
    return w % 7
}
```