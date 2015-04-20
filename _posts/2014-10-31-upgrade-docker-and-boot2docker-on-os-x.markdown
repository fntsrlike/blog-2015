---
layout: post
title: "Upgrade docker and boot2docker on OS X"
date: 2014-10-31 17:59:58 +0800
comments: true
categories: docker
---

在 OS X 更新 Docker 和 Boot2docker
---------------------------------

在 OS X 安裝 Docker 和 Boot2docker 有兩種方式，一種是下載 *.pkg 進行安裝，一種是使用 homebrew 進行安裝。本文前面會描述兩者更新的方式，然後說明如何把 Boot2docker 的 VM Image 更新，也就是把 Docker Server 更新到新版。

<!-- more -->

## Step1: Turn Off Boot2docker

```bash
$ boot2docker stop
```

## Step2: Upgrade Boot2docker

依照您安裝 boot2docker 的方式進行更新

#### Homebrew

```bash
$ brew update
$ brew upgrade docker
$ brew upgrade boot2docker
```

#### Packge Installer

1. 到 [boot2docker/osx-installer](https://github.com/boot2docker/osx-installer/releases) 下載最新版本的安裝檔。
2. 點擊安裝檔進行安裝。


## Step3: Upgrade Boot2docker Image

按照「正常程序」升級映像檔即可。

官網是說如果你是 `0.11.1-pre1` 之前的版本，建議刪除原有映像檔，但是這已經是很早之前的版本了。所以除非有什麼無法升級的意外，才需要「刪除原有映像檔」的方式更新。

#### 正常程序
```bash
$ boot2docker stop
$ boot2docker download
$ boot2docker up
```

#### 刪除原有映像檔
```bash
$ boot2docker stop
$ boot2docker delete     // 注意：本指令會刪除現有的VM映像檔
$ boot2docker download
$ boot2docker init
$ boot2docker up
```

## Step4: Check Version
確認你的版本是否都為最新版了。寫本文時最新版是1.3.0。

```bash
$ boot2docker version
Boot2Docker-cli version: v1.3.0
Git commit: deafc19

$ docker version
Client version: 1.3.0
Client API version: 1.15
Go version (client): go1.3.3
Git commit (client): c78088f
OS/Arch (client): darwin/amd64
Server version: 1.3.1
Server API version: 1.15
Go version (server): go1.3.3
Git commit (server): 4e9bbfa
```

## Reference
1. [Installing Docker on Mac OS X #Upgrading](http://docs.docker.com/installation/mac/#upgrading)
2. [UPGRADE DOCKER AND BOOT2DOCKER ON OSX](http://blog.javabien.net/2014/03/17/upgrade-docker-and-boot2docker-on-osx/)
