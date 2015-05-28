---
layout: post
title: "Docker 初探，實驗室中的運貨鯨"
subtitle: "Docker Talks on NOS of NCU"
date: 2015-5-28 22:36:26 +0800
comments: true
categories: [it]
tags: [docker]
---

敝人有幸在 2015 年 5 月 28 日（週四）晚上，受中大網路開源社之邀的演講。本文所附的即是這場演講所使用的簡報，裡面內容也是針對這次演講所準備。

- 活動：[NOS - KKTIX](http://nos.kktix.cc/events/d1fe178c)
- 簡報：[SpeakerDeck](https://speakerdeck.com/fntsrlike/docker-chu-tan-shi-yan-shi-zhong-de-yun-huo-jing), [SlideShare](http://www.slideshare.net/ruoshiling/docker-48710374)

<div style="width:400px;">
<script async class="speakerdeck-embed" data-id="1db7881143ce43c88b79c396423f6bd0" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
</div>
<span/>

本次演講主要是針對實驗室中，想要使用 Docker 去架設簡單服務的需求，所做的一個 docker 入門講座。和以往講比較詳細部分有所不同，只針對需要使用的指令和概念去講。這次簡報也以比較明亮色系為風格，並且盡量不去使用清單和冗長的文字去做講述，而是以示意圖的方式搭配講說向聽眾解釋。但因時間有限，尚未將以前簡報的素材轉換過來，所以沿革和觀念的部分，還是會先跳到之前的簡報，搭配圖表解說。希望日後有時間將這份簡報編寫完善，然後在有機會受邀演講時，能搭配完善的簡報作為教材，讓更多人了解 Docker。 =D

<!-- more -->

#### 台灣相關社群與同好

如果對 Docker 有興趣，歡迎機如 Docker Taipei 的臉書社團和 Slack 討論平台。

- [FB Club](https://www.facebook.com/groups/docker.taipei/)
- [Slack 平台 - 自己的邀請自己拿](https://ruoshi1.typeform.com/to/bCZMsw)

在中央大學的同學或在桃園、中壢地區的朋友們，歡迎加入或追蹤網路開源社一起討論，這裡匯集滿多中大對資訊有興趣的同學，一起來當好朋友唄。另外幫忙<u>桃園 RoR 讀書會</u>打廣告，在桃園對 RoR 有興趣的朋友也可以加入他們唷。

- [中央大學網路開源社](https://www.facebook.com/NCUNOS)
- [桃園Ruby on Rails讀書會](https://www.facebook.com/groups/tyror/)

#### 演講相關資料

有關 Docker 的連結：

- [Docker](https://www.docker.com/)
- [Docker Hub (Registry)](https://registry.hub.docker.com/)
- [Docker Docs](https://docs.docker.com/)
- [Docker GUI tool - Kitematic](https://github.com/kitematic/kitematic)
- [《Docker —— 從入門到實踐­》正體中文版 電子書](https://www.gitbook.com/book/philipzheng/docker_practice/details)
- [《Docker入門與實戰》實體書](http://www.books.com.tw/products/0010676115)

本場演講所使用的範例，皆已列出其在 Docker Hub 上的連結：

- [WordPress](https://registry.hub.docker.com/_/wordpress/)
- [Ubuntu](https://registry.hub.docker.com/_/ubuntu/tags/manage/)
- [MySQL](https://registry.hub.docker.com/_/mysql/)
- [GitLab](https://registry.hub.docker.com/u/sameersbn/gitlab/)
- [Redmine](https://registry.hub.docker.com/u/sameersbn/redmine/)
- [Discourse](https://github.com/discourse/discourse_docker)
- [Minecraft](https://registry.hub.docker.com/u/itzg/minecraft-server/)

同學若想要研究，可以參考 [讓學生透過 DigitalOcean 嘗試 Docker！](http://blog.fntsr.tw/articles/2014/12/11/student-docker-resource/) 去申請 DigitalOcean 拿額度開機器來玩。
