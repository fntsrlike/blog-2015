---
layout: post
title: "How to build Discourse with docker"
date: 2014-11-07 15:15:59 +0800
comments: true
categories:
---

如何使用 Docker 架設 Discourse
----------------------------

Discourse 是一個使用 Ruby on Rails 編寫的開源論壇程式。與傳統論壇以看版（Boards）為單位去收束文章的方式不同，他是直接使用分類（Categories）作為篩選，讓你去檢視你想要看的文章。這種方式比較適合作為文章性質相近的討論平台，然後再去做比較細的分類。例如：「[新． g0v 後勤中心](http://community.g0v.tw/)」就是討論有關零時政府的專案開發、或是「[RailsFun](http://railsfun.tw/)」則專門針對 Ruby、Ruby on Rails 做手把手教學的討論與問答。

會特別以這篇論壇作為教學題材，除了它本身真的滿好用以外，重要得是官方有提供 Docker 支援！它讓我們可以輕鬆使用它寫好的設定，去架設 Discourse ，甚至同時架設數個都輕而易舉！官方都如此貼心了，那我們還不來試試嗎？

<!-- more -->

# Requirement
在開始前，你必須先安裝下列項目，相關安裝方法，網路上已經滿多教學文了，可以喂狗問問。因為我們是把論壇架設在 Container 中，所以你不需要再去安裝 Ruby on Rails 或其他開發工具。

- Git
- Docker

# Before Begining
在開始前，先做個說明。與官方教學文件不同，我這裡會將一些沒有提到可以改的地方做修改，讓讀者知道原來那邊是可以更換的，而不是以為「我只能這樣做」。比較大的改變如下，希望這些小改變能讓讀者懂得更加彈性的架設論壇。

- 官方的安裝路徑是在`/var`下，本文則是使用`/srv`。
- 官方的設定檔名稱是`app.yml`，本文則是用`childish.yml`。
- 官方的 Port 設定是將 container `port 80` 對應到 host `port 80`，本文改成對應到 host `port 10080`。

# Step1: Install Discourse
首先，我們要把官方寫好的工具 [discourse/discourse_docker](https://github.com/discourse/discourse_docker) `git clone` 下來。然後複製一份設定檔範例到該專案的 containers 資料夾下。

```bash
# 先移動到你想要放置專案的資料夾，通常會是 /var ，本例則是使用 /srv
~ $ cd /srv

# 將 discourse_docker 專案 git clone 下來，並將資料夾名稱改為 discourse
/srv $ git clone https://github.com/discourse/discourse_docker.git discourse
/srv $ cd discourse

# 把 All in one 的設定範例複製一份到 container 資料夾，並改為自己想要的名稱，這裡是用`childish`。
/srv/discourse $ cp samples/standalone.yml containers/childish.yml
```

# Step2: Edit Configuration
接下來我們要編輯設定，可以使用你熟悉的軟體去做編輯，這邊是使用`vim`。
```bash
/srv/discourse $ vim containers/childish.yml
```

這裡會依照原本設定項目的順序，把需要修改的部分列出來。並且已做修改，可以和原值做比較。


{% highlight yaml linenos%}
## /srv/discourse_docker/containers/childish.yml

## 設定你要輸出的 Port，可以配合你網頁伺服器的設定，這裡是將`10080`對到`80`
## 若不想使用網頁伺服器，可以直接將`80`對到`80`，這樣就可以直接讀取你的 domain 做拜訪。
expose:
  - "10080:80"  # 把 host 的 port 10080 轉到 container 的 port 80 (http)
  - "2222:22"   # 把 host 的 port 2222 轉到 container 的 port 22 (ssh)

## 分給資料庫的記憶體，如果你的記憶體有 1 GB，設定 128MB ，若有 4GB ，則建議設為 1GB
db_shared_buffers: "256MB"

## Unicorn 的 workers 數量，如果你的記憶體有 1 GB ，則設定 2：若是有 2 GB ，則建議 3 或 4。
UNICORN_WORKERS: 3

## 設定 DISCOURSE_DEVELOPER_EMAILS 為您的 Email，記得加單引號。
DISCOURSE_DEVELOPER_EMAILS: 'diz@childish.tw'

## 設定 DISCOURSE_HOSTNAME 為您的 hostname ，記得加單引號。
## 若只是架設在本機上，可以寫 localhost
DISCOURSE_HOSTNAME: 'diz.childish.tw'

## 設定郵件伺服器資訊，這裡很重要，若是沒設定好就無法使用論壇。非常不建議直接使用 Gmail。
## 建議：這部分可以使用 Mandrill 的服務，詳細可參見本文的 Mail Test 說明。
DISCOURSE_SMTP_ADDRESS: smtp.mandrillapp.com       # 必填
DISCOURSE_SMTP_PORT: 587                           # 選填
DISCOURSE_SMTP_USER_NAME: diz@childish.tw          # 選填
DISCOURSE_SMTP_PASSWORD: MANDRILL_APP_PASSWORD     # 選填

## 這裡是放置你論壇資料的目錄，包括 Database。
## 將 /var/discourse/ 改為你專案的路徑，就是你 git clone 的目錄。這裡是改 /srv/discourse/。
## 建議：可以把原本的 standalone 改成你的設定檔名稱或 hostname，讓以後要架多重論壇時可以方便管理。這裡是改成 diz.childish.tw
volumes:
  - volume:
      host: /srv/discourse/shared/diz.childish.tw
      guest: /shared
  - volume:
      host: /srv/discourse/shared/diz.childish.tw/log/var-log
      guest: /var/log
{% endhighlight %}

# Command
在你`git clone`下來的專案根目錄中，有一個檔名為`launcher`的腳本執行檔，它可以幫助我們快速使用 docker 架設論壇。在該資料夾下，使用`./launcger`去呼叫。

```
/srv/discourse $ ./launcher
Usage: launcher COMMAND CONFIG [--skip-prereqs]
Commands:
    start:      Start/initialize a container
    stop:       Stop a running container
    restart:    Restart a container
    destroy:    Stop and remove a container
    enter:      Use nsenter to enter a container
    ssh:        Start a bash shell in a running container
    logs:       Docker logs for container
    mailtest:   Test the mail settings in a container
    bootstrap:  Bootstrap a container for the config based on a template
    rebuild:    Rebuild a container (destroy old, bootstrap, start new)

Options:
    --skip-prereqs   Don't check prerequisites
```

它的使用方式就是程式`./launcher`，接著一個 COMMAND 參數，最後加上你的設定檔名稱（不含`.yml`）。在接下來的步驟中，會一一提及各 COMMAN 的使用時機。在這裡先知道它的使用方法就好。

# Step3: Mail Test
在設定檔裡有提到 Mail Server 的設定很重要，這是因為在申請會員時，會寄信請你啟用帳號，否則就無法使用。而這也包括了我們要建立的第一個帳號，管理員帳號。若是這部分設定錯誤，將會導致連管理員帳號都無法登入的窘境。為了讓你知道設定是否正確，這個工具也提供了測試的程式，讓你在建立論壇前，先寄一封信自己，以測試設定是否正常。

在下指令後，他會要求你輸入要寄送的信箱位址，填寫後送出即可。若是有收到信，就代表你的設定是正常的囉。

```bash
# launcher mailtest <config_name>
/srv/discourse $ ./launcher mailtest childish
Enter your email address: diz@childish.tw
DISCOURSE_SMTP_ settings:
 DISCOURSE_SMTP_PASSWORD = (hidden)
 DISCOURSE_SMTP_USER_NAME = diz@childish.tw
 DISCOURSE_SMTP_ADDRESS = smtp.mandrillapp.com
 DISCOURSE_SMTP_PORT = 587

You are correctly configured to use: Mandrill
Success!
```

因為這隻程式會使用到 python 去讀取 yml ，若是你的伺服器環境有缺 `python3-yaml`這個套件，他會提示你去安裝，按照他給的訊息去安裝缺的套件就可以了。若你的作業系統是 Debian / Ubuntu ，可以下 `sudo apt-get install python3-yaml` 去安裝。

## Mandrill
若是你沒有自己的 Mail Server ，可以去申請 [Mandrill](https://mandrill.com/) 的服務。它是一個免費的 Mail Server 服務，特別針對網站系統信件的部分。他會提供您去建立多組 SMTP & API Credentials ，讓我們減少泄漏帳號密碼的危險（這也是我不建議直接使用 Gmail的原因，而且還會時常無法連線，讓你收不到確認信，囧）。總之，若沒有自己的 Mail Server ，就去申請吧！

# Step4: Bootstrap
設定檔編輯好、Mail Servrt 測試後，可以來產生 image 了。這裡使用`bootstrap`去建立。ˊ這部分會需要花一段時間，取決伺服器的網路速度和效能。

```bash
# launcher bootstrap <config_name>
/srv/discourse $ ./launcher bootstrap childish
.........
```

這邊若是遇到無法解析 github.com 的錯誤，可以參考 [Docker Can’t Resolve github.com](/articles/2014/11/08/docker-cant-resolve-github-dot-com/) 這篇文章去解決。

產生成功後，可以用`docker images`做確認。程式會產生以`local_discourse/<config_name>`格式為命名的 image 。

```bash
$ docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
local_discourse/childish   latest              61004de94a0c        22 hours ago        1.489 GB
```

# Step5: Start
有了 image 後，就使用`start`來初始化 contianer 吧。

```bash
# launcher start <config_name>
/srv/discourse $ ./launcher start childish
No cid found, creating a new container
Calculated ENV: .............. # Your enveironment setting
945342195fc05cbfa706f3d1875ab6383fbf5d21a73488367908d9ece21e1abd
```

接著，我們可以使用`docker ps`做確認。程式會產生以你設定檔名稱命名的 container。若是 STATUS 是顯示 Up 即代表成功了！

```bash
/srv/docker/discourse $ docker ps
CONTAINER ID        IMAGE                             COMMAND       CREATED         STATUS         PORTS                                         NAMES
945342195fc0        local_discourse/childish:latest   "/sbin/boot"  52 seconds ago  Up 51 seconds  0.0.0.0:2222->22/tcp, 0.0.0.0:10080->80/tcp   childish
```

# Step6: Browse
最後，您就可以透過你前面坐的設定來瀏覽網站啦。若是在本機可以拜訪 `http://localhost` ，或是去拜訪該伺服器的 hostname 。若 container 的 port 80 不是對應到 host 的 port 80 ，記得加上 port。

以本範例來說就是拜訪 `http://diz.childish.tw:10080`。會這樣做設定，是因為我會再透過 nginx 去監聽 10080 port，讓後 bind 到透過 `diz.childish.tw` 訪問伺服器的連線。當然，這是延伸應用了。

之後，就是 Discourse 相關的操作了，也不在本文的範疇內，按照網站上，官方給的提示去做就行啦。

架設出來大致就如同 [http://diz.childish.tw/](http://diz.childish.tw/)。希望大家都能架設成功囉！

# 後話
這是第一篇 Docker 實例應用的教學文，推廣性質還是比較重，主要還是讓讀者能夠跟著步驟，透過 Docker 將論壇建立起來。之後會想再寫一篇延伸，大概是關於使用 `launcher` 的其他管理，以及如何備份資料以及搬遷，展現使用 Docker 的靈活性。還請大家期待囉。

# Reference
- [Discourse](http://www.discourse.org/)
- [discourse/discourse_docker](https://github.com/discourse/discourse_docker)
- [How To Install Discourse on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-discourse-on-ubuntu-14-04)
- [discourse / docs / INSTALL-digital-ocean.md](https://github.com/discourse/discourse/blob/master/docs/INSTALL-digital-ocean.md)
