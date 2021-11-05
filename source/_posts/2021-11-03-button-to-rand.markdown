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
    
Dim p1, p2 As Integer
p1 = rand()
p2 = rand()

CommandButton1.Enabled = False

For i = 1 To 1500
    If p1 >= i Then
        工作表1.Range("B9") = Race(Int((8) * Rnd()))
    End If
    If p2 >= i Then
        工作表1.Range("H9") = Race(Int((8) * Rnd()))
    End If
Next i

CommandButton1.Enabled = True
End Sub

Private Function rand() As Integer
    Min = 500
    Max = 1500
    rand = Int((Max - Min + 1) * Rnd() + Min)
End Function


```