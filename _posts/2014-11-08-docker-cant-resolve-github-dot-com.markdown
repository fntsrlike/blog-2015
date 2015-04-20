---
layout: post
title: "Docker can't resolve github.com"
date: 2014-11-08 02:30:47 +0800
comments: true
categories: docker
---

Docker 無法解析 github.com
--------------------------

在 Ubuntu 環境下，使用 Docker 架設 Discourse 時遇到了問題，錯誤訊息如下：

```
fatal: unable to access 'https://github.com/SamSaffron/pups.git/': Could not resolve host: github.com
fb4e120a8b107f0ec1e07b3e21a3a1f31e3a5879d30da65242e0333b30533efa
FAILED TO BOOTSTRAP
```

這個問題的是 DNS 相關的錯誤，我們只要幫 Docker 指定 DNS Server 即可。解決辦法依照你安裝 Docker 的方式而異。

<!-- more -->

### via Ubuntu Package

首先，打開 docker 的設定檔。

```bash
$ vim /etc/default/docker
```

然後，將下面這行取消註解。

```bash
DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"
```

最後，重啟 Docker Server

```bash
$ sudo service docker restart
```

### via Binary

如果你是透過二進位檔案執行 Docker server，你只需在啟動 Docker daemon 時，加上 DNS 參數即可。如下：

```bash
$ sudo docker -d -D --dns 8.8.8.8 --dns 8.8.4.4 &
```


`8.8.8.8`和`8.8.4.4`都是 Google 的 DNS，你也可以增修你喜歡的 DNS Server。

# Reference
- [How To Install Discourse on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-discourse-on-ubuntu-14-04)
- [A fatal: unable to access ‘https://github.com/SamSaffron/pups.git/’: Could not resolve host: github.com](https://meta.discourse.org/t/afatal-unable-to-access-https-github-com-samsaffron-pups-git-could-not-resolve-host-github-com/18611)
- [Docker - Network calls fail during image build on corporate network](http://stackoverflow.com/questions/24151129/docker-network-calls-fail-during-image-build-on-corporate-network)
- [鳥哥的 Linux 私房菜 - 第十九章、主機名稱控制者： DNS 伺服器](http://linux.vbird.org/linux_server/0350dns.php#DNS_resolver_file)
