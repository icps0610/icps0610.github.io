---
layout: post
title: "days_excel"
date: 2018-10-14 15:47:25 +0800
comments: true
categories: excel  
---

<pre>
A1
1999/12/29

C1
=LEFT(A1, SEARCH("/",A1,1)-1)

D1
=MID(A1,SEARCH("/",A1,1)+1,SEARCH("/",A1,SEARCH("/",A1,1)+1)-SEARCH("/",A1,1)-1)

E1
=RIGHT(A1,LEN(A1)-SEARCH("/",A1,SEARCH("/",A1,1)+1))

F1
=(C1-1)*365+INT((C1-1)/4)-INT((C1-1)/100)+INT((C1-1)/400)+DATEDIF("1/1",D1&"/"&E1,"YD")+IF(OR(MOD(C1,400)=0,AND(MOD(C1,4)=0,MOD(C1,100)<>0)),IF(INT(D1)>2,2,1), 1)

</pre>>