---
layout: post
title: "button_to_rand"
date: 2021-11-03 20:28:59 +0800
comments: true
categories: excel
---


```
Private Sub CommandButton1_Click()

Dim min As Integer, max As Integer, p1 As Integer, p2 As Integer

min = 300
max = 1500

p1 = rand(min, max)
p2 = rand(min, max)

CommandButton1.Enabled = False

For i = 1 To 1500
    If p1 >= i Then
        工作表1.Range("B9") = Race()
    End If
    If p2 >= i Then
        工作表1.Range("H9") = Race()
    End If
Next i

CommandButton1.Enabled = True
End Sub

Private Function rand(min As Integer, max As Integer) As Integer
    rand = Int((max - min + 1) * Rnd() + min)
End Function

Private Function Race() As String
    ype = Array("英格蘭", "中國", "法蘭西", "羅馬", "蒙古", "羅斯", "蘇丹", "阿拔斯")
    Race = ype(Int((8) * Rnd()))
End Function

```