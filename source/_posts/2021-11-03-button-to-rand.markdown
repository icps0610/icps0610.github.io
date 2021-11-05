---
layout: post
title: "button_to_rand"
date: 2021-11-03 20:28:59 +0800
comments: true
categories: excel
---


```
Private Sub CommandButton1_Click()

Race = Array("英格蘭", "中國", "法蘭西", "羅馬", "蒙古", "羅斯", "蘇丹", "阿拔斯")

min = 400
max = min * Int(2 * Rnd() + 2)
mmax = max

If Rnd() > 0.5 Then
    temp = max
    max = min
    min = temp
End If

CommandButton1.Enabled = False

For i = 1 To mmax
    If min >= i Then
        工作表1.Range("D9") = Race(Int((8) * Rnd()))
    End If
    If max >= i Then
        工作表1.Range("I9") = Race(Int((8) * Rnd()))
    End If
Next i

CommandButton1.Enabled = True
End Sub





```