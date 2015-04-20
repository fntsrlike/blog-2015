---
layout: post
title: "How to deploy PHP projects with docker"
date: 2014-10-18 17:00:00 +0800
comments: true
categories: docker php
---

本簡報是為PHPConf2014議程所準備的，但陸續會在針對簡報做維護，並且在這邊回答會眾對於當天大會聽講但是沒有聽懂的部分。

- 影片： (請耐心等候主辦單位釋出)
- 簡報： [SpeakerDeck](https://speakerdeck.com/fntsrlike/how-to-deploy-php-projects-with-docker) | [SlideShare](http://www.slideshare.net/ruoshiling/how-to-deploy-php-projects-with-docker)

<div style="width:400px;">
<script async class="speakerdeck-embed" data-id="4be222a03e440132e5ed2a2ba31f9bce" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
</div>
<!-- more -->

## Updated Log

### 2014-10-25 Sat. | v2.1
- 因為SpeakDeck原本的項目一直無法轉檔成功，於是另外再開一個項目重新上傳，網址不變。

### 2014-10-24 Fri. | v2
- 修正演講時發現的小錯。
- 補上缺少的Expose Port的投影片。

## Feedback
- 講起來太快，很像在趕行程。
- 實戰經驗說的不足。缺少了運用在正式伺服器上面的相關實作經驗
- 缺少實務面 Scalability in Mind
- 講者還只是學生沒有真正在軟體業待過，對軟體公司複數伺服器的自動化部署應該是完全沒有概念。
- AUFS的理解也不太正確。
- LXC是Docker 0.38的限制，Docker 1.0已經開發了自己的container lib而不再以linux container為預設值。


## Q & A
