---
layout: post
title: "googleForms"
date: 2021-06-01 13:52:18 +0800
comments: true
categories: golang
---

``` go  
//"github.com/sclevine/agouti" 

func main() {
    page, driver := openBrowser(600, 700)
    defer driver.Stop()
    page.Navigate(url)
    sleep(1000)
    delay := 100

    list = page.AllByClass("freebirdFormviewerViewNumberedItemContainer")

    fillAnswer("select", "業務支援部")
    
    radios := strings.Split("211110021111", "")
    for _, radio := range radios {
        fillAnswer("radio", radio)
    }

    page.AllByClass("appsMaterialWizButtonPaperbuttonLabel").At(0).Click()

}

func fillAnswer(cate, ans string) {
    switch cate {
    case "text":
        //文字
        list.At(idx).FirstByClass("quantumWizTextinputPaperinputInput").Fill(ans)
    case "select":
        //下拉選單
        list.At(idx).FirstByClass("quantumWizMenuPaperselectOption").Fill(ans)
    case "radio":
        //選擇
        i, _ := strconv.Atoi(ans)
        list.At(idx).AllByClass("appsMaterialWizToggleRadiogroupOffRadio").At(i).Click()
    }
    idx += 1
    sleep(delay)
}

```  