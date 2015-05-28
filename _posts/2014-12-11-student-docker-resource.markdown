---
layout: post
title: "讓學生透過 DigitalOcean 嘗試 Docker！"
subtitle: "Let Students try Docker at DigtialOcean!"
date: 2014-12-11 01:20:58 +0800
comments: true
categories: [it]
tags: [docker, vps]
description: "Docker 是最近正夯的輕量化虛擬技術，也是我希望能多多推廣的。適逢最近要去[台科大程式設計社](http://ntustcoding.club/)演講，希望在演講時，聽眾也能在下方一起操作。但是一項技術在推廣時，第一個遇到的門檻就是安裝，為了避免在網路上已經很多教學文的安裝花費太多時間，所以希望能讓聽眾透過 VPS 去使用 Docker，所以寫了這邊文章去推廣這種方式。這樣既避免安裝的過程，也讓現場省下許多下載 Image 頻寬，避免網路爆炸。"
---

Docker 是最近正夯的輕量化虛擬技術，也是我希望能多多推廣的。適逢最近要去[台科大程式設計社](http://ntustcoding.club/)演講，希望在演講時，聽眾也能在下方一起操作。但是一項技術在推廣時，第一個遇到的門檻就是安裝，為了避免在網路上已經很多教學文的安裝花費太多時間，所以希望能讓聽眾透過 VPS 去使用 Docker，所以寫了這邊文章去推廣這種方式。這樣既避免安裝的過程，也讓現場省下許多下載 Image 頻寬，避免網路爆炸。

本文章主要是教導如何透過 DigitalOcean 開一個已經有 Docker 的 VPS。在過程中會順便推廣 Github 的 Student Developer Pack 有關 DigitalOcean  100 美金的資源。若不是學生身份，亦可透過本文的連結註冊，得到 10 美金的 Referral 額度。

### Outline

- 註冊 DigitalOcean
- 建立 VPS
- 連線與測試

<!-- more -->

## 註冊 DigitalOcean
開啟 [DigitalOcean](http://goo.gl/P9rn2B) 的首頁，然後在中間的表單填上您的 電子郵件與密碼。

[![][1-1]][1-1]{:target="_blank"}

送出表單後，就會被引導到使用者後台。

[![][1-2]][1-2]{:target="_blank"}

然後我們要去收信，驗證我們的電子郵件。

[![][1-3]][1-3]{:target="_blank"}

如果你是透過上面的連結申請，會因為是透過 referral code link 申請的使用者，而收到另一封信，說您已經得到 10 美金的額度。

[![][1-4]][1-5]{:target="_blank"}

驗證後，您就會被轉到[使用者付款頁面](https://cloud.digitalocean.com/user_payment_profiles)，請你填寫您的信用卡資訊去啟用服務。若是沒有信用卡，亦可請朋友透過 Paypal 幫忙轉五美金的一次性付款去啟用。

[![][1-5]][1-5]{:target="_blank"}

## 申請 Github 的 Student Developer Pack
若是還沒有註冊 [Github]()，請先去註冊一個帳號，然後登入。

接著到 [Github Education]() 的 [Student Developer Pack]() 網頁，點選頁面中間的「Get Your Pack」，或是右上角的 Request a discount。

[![][2-1]][2-1]{:target="_blank"}

點選後會出現表單，在步驟一，只需要選取你的身份和你要把這個優惠用在使用者帳號還是組織帳號。這裏只需選取 Student 和 Individual account 即可。

[![][2-2]][2-2]{:target="_blank"}

步驟二裡，你填寫你的名稱、學校信箱、學校名稱、畢業年份以及你打算怎麼使用Github。這裏都要填寫英文，可去學校首頁找找學校的英文名稱。比較重要的是選擇你的學校信箱，若是你還沒通過這類驗證，你可以點選「[add and verify it](https://github.com/settings/emails)」，去個人後台驗證你的學校信箱。至於如何使用，你就照實填寫就好，像我就是寫 Practice coding and git。

[![][2-3]][2-3]{:target="_blank"}

等認證通過後，到 Student Developer Pack 頁面重新整理，或是再次點選「Get Your Pack」，就可以看到頁面原本的「Get Your Pack」消失了，而改成一個以黃底黑字的「My Pack」。

[![][2-4]][2-4]{:target="_blank"}

然後我們在下方服務中，尋找 DigitalOcean 。你會看到旁邊就會有你的 Promo Code 囉。

[![][2-5]][2-5]{:target="_blank"}

將 Code 複製下來，然後來到[使用者付款頁面](https://cloud.digitalocean.com/user_payment_profiles)，看到 Promo Code 的部分，將我們 Code 貼上，他就會自動認證。

[![][2-6]][2-6]{:target="_blank"}

認證成功後，來到 [Billing](https://cloud.digitalocean.com/billing) 頁面，你就會發現你多了 100 美金的額度，夠讓你使用一陣子。

## 建立 VPS
接著點選左邊導覽列的 [Create](https://cloud.digitalocean.com/droplets/new) ，我們要來開始建立我們 VPS 囉！

[![][3-1]][3-1]{:target="_blank"}

### Droplet Hostname
這裏是填寫你的 VPS 名稱，看你習慣怎麼命名。我自己是把我的 VPS 用魔戒系列人物命名啦。=P

### Select Size
選擇 VPS 的使用方案，一般來說只需使用 5 美金的方案即可。

[![][3-2]][3-2]{:target="_blank"}

### Select Region
選擇地區，也就是選擇 VPS 的機房所在。為了保持較良好的連線速度，選擇位在亞洲的新加坡機房是比較好的。

[![][3-3]][3-3]{:target="_blank"}

### Available Settings
這裏目前不需要設定。若是懂項目意思的可以自行斟酌。

### Select Image
選擇你 VPS 所使用的初始映像檔，並分別有五個分頁。為了讓我們能快速使用 Docker ，我們選擇 Applications 分頁的 Docker 1.3.2 on 14.04 的映像檔。DigitalOcean 會在建立 VPS 時，安裝 Ubuntu 14.04 的作業系統，並在裡面裝好 Docker！

[![][3-4]][3-4]{:target="_blank"}

### Add SSH Keys
增加 SSH 金鑰。這是選填項目，但是基於安全性考量，我極度建議你使用。關於相關說明，可以Google 「[SSH 登入](https://www.google.com.tw/webhp?#newwindow=1&q=SSH+%E7%99%BB%E5%85%A5)」去學習。

## 連線與測試
當 VPS 建立完成後，進到 VPS 狀態頁面，在上方就可以看到自己的 IP。

[![][4-1]][4-1]{:target="_blank"}

因為我們前面有設定 SSH 登入，所以就可以不用打密碼，直接透過 terminal 下 SSH 連線到 VPS。

[![][4-2]][4-2]{:target="_blank"}

最後，在 terminal 輸入`docker version`的指令，若有安裝成功，就會顯示 Docker 的版本資訊囉！

[![][4-3]][4-3]{:target="_blank"}

[1-1]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/1_1.png
[1-2]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/1_2.png
[1-3]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/1_3.png
[1-4]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/1_4.png
[1-5]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/1_5.png

[2-1]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/2_1.png
[2-2]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/2_2.png
[2-3]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/2_3.png
[2-4]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/2_4.png
[2-5]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/2_5.png
[2-6]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/2_6.png

[3-1]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/3_1.png
[3-2]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/3_2.png
[3-3]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/3_3.png
[3-4]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/3_4.png

[4-1]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/4_1.png
[4-2]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/4_2.png
[4-3]: {{ site.url }}/images/posts/2014-12-11-student-docker-resource/4_3.png
