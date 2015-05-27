---
layout: post
title: "在 OS X 上，透過 boot2docker 使用 docker 的 data volume"
subtitle: "Use docker data volume on OS X"
date: 2014-10-31 22:05:15 +0800
comments: true
categories: [it]
tags: [docker]
---

Docker 1.3 在 2014-10-16 釋出。其中，在方便性上最讓人注目的更新，除了 exec 指令以外，就是 boot2docker 在 Mac OS X 資料夾分享功能的改進，本文主要是針對後者去做講述。

<!-- more -->

## 前言

由於 Docker 只支持 Linux 作業系統，倘若要於 OS X 使用 Docker ，會使用 boot2docker 這個工具，在 VirtualBox 上建立一個 boot2docker-vm 的映像檔。然後，透過這個 Linux VM 去操作 Docker。

但是，在 Docker 1.3 之前，因為 boot2docker 的映像檔沒有支援 VirtualBox Guest Additions ，所以無法使用 Virtualbox 分享資料夾的功能，將 OS X 的資料夾掛載到虛擬機器裡。因此必須另外自行製作有支援該 VirtualBox Guest Additions 的映像檔（或是下載別人做好的），然後設定 Virtual Box ，把 OS X 的資料夾自動掛載到虛擬機器裡。

不過，隨著 Docker 1.3 釋出，boot2docker 也一併將這個功能引入。使用者可以直接透過 boot2docker 的映像檔使用這功能，而且它會自動把 `/Users` 資料夾掛載到虛擬機器裡，不需要另外設定！

下圖是在 OS X 上，使用 boot2docker 建立 data volume 的示意圖，希望能幫助各位了解運作原理。

![]({{ site.url }}/images/posts/2014-10-31-use-docker-data-volume-on-os-x-001.png)


## 前置作業

在開始前，必須先安裝或更新 docker 和 boot2docker 到 1.30 以上的版本，更新的方法在我前一篇文章 [Upgrade Docker and Boot2docker on OS X](/articles/2014/10/31/upgrade-docker-and-boot2docker-on-os-x/) 已經說明了，可以參考看看，這邊就不多贅述。

接著在 Terminal 下 `$ boot2docker ssh 'ls -al /Users'` 的指令，確認是否已經成功掛載。如果成功，應該會出現 `/Users` 的目錄。

{% highlight bash %}
$ boot2docker ssh 'ls -al /Users'
drwxr-xr-x    1 docker   staff          204 Mar 29  2014 .
drwxr-xr-x   17 root     root           400 Oct 31 10:29 ..
-rw-r--r--    1 docker   staff            0 Aug 25  2013 .localized
drwxrwxrwx    1 docker   staff          306 Oct 18 04:08 Shared
drwxr-xr-x    1 docker   staff         3774 Oct 31 17:52 user5566
{% endhighlight %}


## Data Volume

本文利用 ubuntu 的 images 去做建立 data volume 的示範。開始前，再次強調，因為 boot2docker 只有把 OS X 的 `/Users` 資料夾掛載到虛擬機上，所以 data volume 的 host 資料夾必須在 `/Users` 底下。

### 首先
把想建立 data volume 的資料夾與檔案準備好。

{% highlight bash %}
$ mkdir ~/docker-volume/test
$ touch ~/docker-volume/test/volume-test.md
$ ls -al ~/docker-volume/test
total 0
drwxr-xr-x  3 user5566  staff  102 11  1 02:01 .
drwxr-xr-x  3 user5566  staff  102 11  1 02:01 ..
-rw-r--r--  1 user5566  staff    0 11  1 02:01 volume-test.md
{% endhighlight %}

### 接著
建立一個 Container 。

並且，將剛才建立的資料夾，透過 data volume 掛載到 `/volume_test` 的位置。並且確認是否成功。

{% highlight bash %}
# 建立 Container
$ docker run -it -v ~/docker-volume/test:/volume_test ubuntu:latest /bin/bash
root@d0d097097657:/#

# 測試 data volume 是否成功
root@d0d097097657:/# ls -al /volume_test/
total 4
drwxr-xr-x  1 1000 staff  102 Oct 31  2014 ./
drwxr-xr-x 50 root root  4096 Oct 31 17:51 ../
-rw-r--r--  1 1000 staff    0 Oct 31  2014 volume-test.md
{% endhighlight %}

### 最後
在 OS X 和 Container 中，都建立一個檔案，以測試同步與否。

{% highlight bash %}
# OS X
$ touch ~/docker-volume/test/file-from-osx.md

# Container
root@d0d097097657:/# touch /volume_test/file-from-container.md
{% endhighlight %}

看起來是成功了，歡呼囉！

{% highlight bash %}
# OS X 確認
$ ls -al ~/docker-volume/test
total 0
drwxr-xr-x  5 user5566  staff   170B 11  1 02:12 .
drwxr-xr-x  3 user5566  staff   102B 11  1 02:01 ..
-rw-r--r--  1 user5566  staff     0B 11  1 01:58 file-from-container.md
-rw-r--r--  1 user5566  staff     0B 11  1 02:12 file-from-osx.md
-rw-r--r--  1 user5566  staff     0B 11  1 02:01 volume-test.md

# Container 確認
root@d0d097097657:/# ls -al /volume_test/
total 4
drwxr-xr-x  1 1000 staff  170 Oct 31  2014 .
drwxr-xr-x 50 root root  4096 Oct 31 17:51 ..
-rw-r--r--  1 1000 staff    0 Oct 31 17:58 file-from-container.md
-rw-r--r--  1 1000 staff    0 Oct 31  2014 file-from-osx.md
-rw-r--r--  1 1000 staff    0 Oct 31  2014 volume-test.md
{% endhighlight %}

## 後記
Data Volume 是 docker 在應用 LXC 時，一個非常重要的功能。在初學 Docker 前，因為不懂 boot2docker 的運作原理，在這功能鬼打牆好多次，都無法成功。後來知道原理後，卻覺得在 OS X 實作這功能太麻煩了，改用 VPS 直接用 Linux 去玩 Docker 。還好，現在 boot2docker 已經做好這件事了，讓我們可以更快樂的在 OS X 上面玩 Docker ，尤其是進行開發啦！(rock)

剛好，這塊是我在 [PHPConf 演講](/articles/2014/10/18/how-to-deploy-php-projects-with-docker/) 裡，只有稍微帶過的部分，希望這篇文章能補足當時因為時間關係，而沒講明的部分。在之後，我也會把握空閒時間，多寫幾篇有關 Docker 的文章，補足演講的缺口，希望大家會喜歡。 =D

最後，祝各位在 OS X 上，愜意地 Docker 囉！


## Reference
<span/>

- [boot2docker together with VirtualBox Guest Additions: How to mount /Users into boot2docker](https://medium.com/boot2docker-lightweight-linux-for-docker/boot2docker-together-with-virtualbox-guest-additions-da1e3ab2465c)
- [Managing Data in Containers](https://docs.docker.com/userguide/dockervolumes/)
- [DOCKER 1.3: SIGNED IMAGES, PROCESS INJECTION, SECURITY OPTIONS, MAC SHARED DIRECTORIES](https://blog.docker.com/2014/10/docker-1-3-signed-images-process-injection-security-options-mac-shared-directories/)
- [boot2docker/boot2docker](https://github.com/boot2docker/boot2docker)
- [Volumes: vboxguest + vboxsf modules #282](https://github.com/boot2docker/boot2docker/issues/282#issuecomment-44601104)
- [Post of Docker.Taipei](https://www.facebook.com/groups/docker.taipei/permalink/1522080854693939/)
