---
layout: post
title: "vba_excel_csv_and_use_ruby_read"
date: 2018-04-21 19:43:25 +0800
comments: true
categories: ruby
---

### excel vba => Unicode => Icov => utf-8

``` 
#excel vba
Dim i As Integer
Dim WS_Count As Integer

file = "z:\workbook.xlsx"
csv = "z:\workbook.csv"
  
WS_Count = ActiveWorkbook.Worksheets.Count
For i = 1 To WS_Count
Dim ws As Worksheet

Set ws = ThisWorkbook.Worksheets(i)
    ws.Copy
    ActiveWorkbook.SaveAs Filename:=csv, _
        FileFormat:=xlUnicodeText
Next i
Application.DisplayAlerts = False
ActiveWorkbook.Close
Shell ("ruby X:\excel\send.rb")

Do
    If Dir(file) <> "" Then
        Exit Do
    End If
    DoEvents 'Prevents Excel from being unresponsive
    Application.Wait Now + TimeValue("0:00:01") 'wait for one second
Loop
Workbooks.Open file
```

``` ruby
# encoding: utf-8
require "net/ssh"
Net::SSH.start(host, user, :password => passwd ) do |ssh|
    puts ssh.exec! "ruby excel.rb"
end
```

```ruby 
require 'csv'
require 'iconv'
file_path = "/tmp/workbook.csv"
f = IO.read(file_path)
Iconv.iconv('utf-8', 'unicode', f).join.split("\r\n")
```

