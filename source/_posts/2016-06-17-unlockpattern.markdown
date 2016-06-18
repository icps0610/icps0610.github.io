---
layout: post
title: "unlockpattern"
date: 2016-06-17 00:56:20 +0800
comments: true
categories: ruby
---

``` ruby
#!/usr/bin/env ruby
# encoding: utf-8
require 'rmagick'
class Up

  def all_possible
    (4..9).map do |i|
      @ok ||= 0
      "123456789".scan(/./).permutation(i).to_a.each do |c|
        @ok+=1 if check(c.to_a.join,0) == true
      end
    end
    return @ok
  end

  def rand_produce
    @canvas = Magick::ImageList.new
    @canvas.read("base.jpg")
    @draw = Magick::Draw.new
    code = produce_code
    draw_line(code)
    save(code)
  end

  def produce_code
    code="123456789".scan(/./).sample(9).to_a.join
    check(code,0)? code : produce_code
  end

  def check(code,i)
    if i == code.length-1
      return true
    else
      x=(code[i].to_i-1).divmod(3)
      y=(code[i+1].to_i-1).divmod(3)
      testx = (x[1]-y[1]).abs
      testy = (x[0]-y[0]).abs
      if (testx-testy).abs ==1
        check(code,i+1)
      elsif testx == 2 or testy == 2
        m = (code[i].to_i+code[i+1].to_i)/2
        code[0,i].match(m.to_s) == nil ? false : check(code,i+1)
      else
         check(code,i+1)
      end
    end
  end

  def location(i)
    base=i.to_i.divmod(3)
    if base[1] == 0
      x=base[1]+2
      y=base[0]-1
    else
      x=base[1]-1
      y=base[0]
    end
    return [50+100*x-5,50+100*y]
  end

  def draw_line(code)
    (0..code.length-2).each do |i|
      @draw.fill('red')
      @draw.line(location(code[i])[0],location(code[i])[1],location(code[i+1])[0],location(code[i+1])[1])
      if i == 0
        @draw.fill('blue')
        @draw.circle(location(code[i])[0],location(code[i])[1],location(code[i])[0]+6,location(code[i])[1]+6)
      elsif i == code.length-2
        @draw.fill('yellow')
        @draw.circle(location(code[i+1])[0],location(code[i+1])[1],location(code[i+1])[0]+6,location(code[i+1])[1]+6)
      end
    end
  end

  def save(code)
    text = Magick::Draw.new
    text.font_family = 'helvetica'
    text.pointsize = 14
    text.gravity = Magick::CenterGravity
    text.annotate(@canvas, 0,0,-5,-125, code)
    @draw.draw(@canvas)
    @canvas.display
    @canvas.write("code.jpg")
  end
end
unlockpattern=Up.new
puts unlockpattern.all_possible #所有可能
unlockpattern.rand_produce　#隨機產生

```

