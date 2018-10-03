---
layout: post
title: "iptocountry"
date: 2018-10-03 17:54:18 +0800
comments: true
categories: ruby
---
###  http://software77.net/geo-ip/

``` ruby
def prune(itc, idx, tmp, sort)
    tmp.first[1] = itc[idx][1]
    sort << tmp.first
    return sort
end

def reorganize(itc, idx=0, tmp=[], sort=[])
    next_elemnt = itc[idx+1]
    if next_elemnt == nil
        return prune(itc, idx, tmp, sort)
    end
    if not next_elemnt[2] == itc[idx][2] and tmp == []
        sort << itc[idx]
    elsif next_elemnt[2] == itc[idx][2]
        tmp << itc[idx]
    else
        sort = prune(itc, idx, tmp, sort)
        tmp  = []
    end
    return reorganize(itc, idx+1, tmp, sort)
end

file = File.open("sort_iptocountry.txt", 'w')
file.truncate(file.size)
File.readlines("IpToCountry.txt").map do |line|
    l = line.gsub("\"", "").chomp.split(",")
    p line = [l[0], l[1], l[4], l[6]].join(",")
    file.write("#{line}\n")
end
file.close()

```