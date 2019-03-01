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
    co.include? l.to_s and co.include? n.to_s or n.to_i == 10 - l.to_i
end

def num_possible(num, l)
    ((1..9).to_a - num - [l]).map do |n|
        if cross_two_num(l, n)
            m = (l + n)/2
            n if num.include?(m)
        else
            n
        end
    end.compact
end

def dfs(pt, route=[], take=->(r) { yield(r) })
    possible = num_possible(route, pt)
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

def produce_rand(length)
    num = [rand(1..9)]
    length.times do
        num << num_possible(num, num[-1]).sample
    end
    num.join
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
end

def draw_line(num, scnum)
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
    canvas = Magick::Image.new(300, 300)
    text = Magick::Draw.new
    text.font_family = 'TW-Sung'
    text.pointsize = 14
    text.gravity = Magick::CenterGravity
    text.annotate(canvas, 0, 0, -5, -125, num)
    text.annotate(canvas, 0, 0, -5,  140, scnum)
    draw = Magick::Draw.new
    spinner = Magick::ImageList.new
    (1..9).each do |i|
        x, y = location(i)
        draw.circle(x, y, x + 6, y + 6)
        draw.draw(canvas)
    end
    spinner << canvas.copy
    draw.draw(spinner)
    num.split(//).take(num.size-1).each_with_index do |n, i|
        o = num[i + 1]
        draw.fill('red')
        ax, ay = location(n)
        bx, by = location(o)
        draw.line(ax, ay, bx, by)
        if i == 0
            draw.fill('blue')
            draw.circle(ax, ay, ax + 6, ay + 6)
        elsif i == num.length-2
            draw.fill('yellow')
            draw.circle(bx, by, bx + 6, by + 6)
            5.times do
                spinner << canvas.copy
                draw.draw(spinner)
            end
        end
        spinner << canvas.copy
        draw.draw(spinner)
    end
    spinner.delay = 30
    spinner.compression = Magick::LZWCompression
    spinner.write('/tmp/unlockpattern.gif')
end

#===method 1
t = Time.now
all = []
(1..9).map do |i|
    dfs(i) do |num|
        all << num
    end
end
p all.size
p Time.now - t

#===method 2
t = Time.now
a = (4..9).map{|e| "123456789".split(//).permutation(e).map &:join }.flatten

all = a.map do |num|
    num if num_check(num)
end.compact
p all.size
p Time.now - t

#rand
sc = '零一二三四五六七八九'
p num   = produce_rand(9)
puts scnum = num.split(//).map{|i| sc[i.to_i] }.join
draw_line(num, scnum)
```

