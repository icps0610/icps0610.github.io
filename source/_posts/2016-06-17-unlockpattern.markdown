---
layout: post
title: "unlockpattern"
date: 2016-06-17 00:56:20 +0800
comments: true
categories: ruby
---

``` ruby
# encoding: utf-8
require 'rmagick'

def cross_two_num(l, n)
    co = "1379"
    co.include? l.to_s and co.include? n or n.to_i == 10 - l.to_i
end

def num_check(num)
    (0..num.size-2).each do |i|
        l = num[i]
        n = num[i+1]
        if cross_two_num(l, n)
            m = (l.to_i + n.to_i)/2
            return false if not num[0..i].include?(m.to_s)
        end
    end
    return true
end

def num_possible(num, l)
    num = (num + l.to_s).split(//) 
    all = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
    (all - num).map do |n|
        if cross_two_num(l, n)
            m = (l.to_i + n.to_i)/2
            n if num.include?(m.to_s)
        else
            n
        end
    end.compact
end

def dfs(pt, route=[], take=->(r) { yield(r) })
    num = route.join
    possible = num_possible(num, pt)
    if route.size == 8 or possible.size > 0
        route.push(pt)
        if route.size >= 4
            take.call(route.join)
        end
        possible.each do |s|
            dfs(s, route, take)
        end
        route.pop
    end
end

def produce_rand(length, num="", s=rand(1..9))
    length.times do
        num += s.to_s
        s = num_possible(num[0, num.size-1],  num[-1]).sample
    end
    return num
end

def location(i)
    base = i.to_i.divmod(3)
    if base[1] == 0
        x = base[1] + 2
        y = base[0] - 1
    else
        x = base[1] - 1
        y = base[0]
    end
    return [ 50 + 100 * x - 5, 50 + 100 * y ]
end

def draw_line(num)
    canvas = Magick::ImageList.new
    canvas.read("base.jpg")
    text = Magick::Draw.new
    text.font_family = 'helvetica'
    text.pointsize = 14
    text.gravity = Magick::CenterGravity
    text.annotate(canvas, 0, 0, -5, -125, num)
    canvas.write("/tmp/num0.jpg")
    draw = Magick::Draw.new
    (0..num.length-2).each do |i|
        j = i + 1
        draw.fill('red')
        ax, ay = location(num[i])
        bx, by = location(num[j])
        draw.line(ax, ay, bx, by)
        if i == 0
            draw.fill('blue')
            draw.circle(ax, ay, ax + 6, ay + 6)
        elsif i == num.length-2
            draw.fill('yellow')
            draw.circle(bx, by, bx + 6, by + 6)
        end
        draw.draw(canvas)
        canvas.write("/tmp/num#{j}.jpg")
    end
    `convert -delay 50 -loop 0 /tmp/num*.jpg /tmp/unlockpattern.gif`
end

sc = '〇〡〢〣〤〥〦〧〨〩'
p num   = produce_rand(9)
puts scnum = num.split(//).map{|i| sc[i.to_i] }.join
draw_line(num)

t = Time.now
all = []
(1..9).map do |i|
    dfs(i) do |num|
        all << num
    end
end
p all.size
p Time.now - t

t = Time.now
a = (4..9).map{|e| "123456789".split(//).permutation(e).map &:join }.flatten

all = a.map do |num|
    if num_check(num) == true
        num
    end
end.compact
p all.size
p Time.now - t

```

