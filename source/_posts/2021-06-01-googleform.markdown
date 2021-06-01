---
layout: post
title: "googleForm"
date: 2021-06-01 13:52:18 +0800
comments: true
categories: golang
---
``` golang
//"github.com/sclevine/agouti"

page, driver := openBrowser(600, 700)
defer driver.Stop()
page.Navigate(url)
sleep(1000)
delay := 100

//日期
page.AllByClass("quantumWizTextinputPaperinputInput").At(0).Fill(getToday())
sleep(delay)
//下拉式選單
page.AllByClass("quantumWizMenuPaperselectEl").At(0).AllByClass("quantumWizMenuPaperselectOption").At(0).Fill("業務支援部")
sleep(delay)
//文字
page.AllByClass("quantumWizTextinputPaperinputInput").At(2).Fill(earTemp)
sleep(delay)
//選擇
page.AllByClass("appsMaterialWizToggleRadiogroupOffRadio").At(2).Click()
sleep(delay)
//送出
page.AllByClass("appsMaterialWizButtonPaperbuttonLabel").At(0).Click()


```