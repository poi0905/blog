---
layout: post
title: 如何用github page製作屬於自己的部落格
summary: 這篇文章來分享一下如何用github page來做屬於自己的部落格(也怕自己以後忘記...)
featured-img: emile-perron-190221
categories: [教學]
---

# 前言

***

其實youtube上已經有許多影片分享如何製作github page，

如果想要跟著做的可以先參考這[影片](https://www.youtube.com/watch?v=BA_c3bGQXlQ&t=96s)。

開始變化的地方是影片中的選取主題處，

由於github page上的主題選擇不多，

所以我們可以從[這裡](http://jekyllthemes.org/)來選擇更豐富的主題，

選擇完就要開始動工了!



# 實作

***

## 第一步

你要先下載[Git Bash](https://git-scm.com/downloads)，

下載完成後打開它

![image](https://raw.githubusercontent.com/poi0905/blog/master/assets/img/posts/1.png)

## 第二步

你必須將你的程式碼選擇在一個資料夾存放，以我為例，我是放在**大學資料**中的Blog的資料夾中，

因此，我會在我的bash打:

1. cd 'OneDrive - g.ntu.edu.tw'
2. cd 大學資料
3. cd blog

簡單來說就是進入到你要存放的地方。

## 第三步

把你想要的主題 clone 進去，方法是在 bash 中打 **git clone 網址**

## 第四步

clone完成後，把資料移到屬於你自己的資料夾，並且打開自己的編譯器，找到**_config**，改裡面的url。

以我為例，我是這樣:

baseurl: "/blog"

url: "https://poi0905.github.io"

## 第五步

接者在 bash 裡依序打下列指令:

1. git add .
2. git commit -m"_隨意(可以輸日期、或做了什麼事情)_"
3. git push

## 第六步

基本上，這樣就大功告成了。

不過要注意的是並不是每個theme都是這樣改，有些有不同的改法，或者不用改，這些都是有可能的。

因此有問題的話可以問我(我有能力的話XD)

**記得隨時要注意自己目前在什麼位子喔!**

下面放個圖當作參考

![image](https://raw.githubusercontent.com/poi0905/blog/master/assets/img/posts/2.jpg)

![image](https://raw.githubusercontent.com/poi0905/blog/master/assets/img/posts/3.jpg)

![image](https://raw.githubusercontent.com/poi0905/blog/master/assets/img/posts/4.jpg)



