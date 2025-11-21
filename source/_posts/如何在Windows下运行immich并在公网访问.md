---
title: 如何在Windows下运行immich并在公网访问
date: 2025-11-22 10:02:00
tags:

---

高效利用手上的垃圾硬件

![](https://cdn.jsdelivr.net/gh/hachiroku-lose/hachiroku-lose.github.io@image/blog-image/immich%E5%BE%BD%E6%A0%87.svg)

<!-- more -->

注意!在阅读本文之前请务必先阅读[immich的官方文档](https://docs.immich.app/overview/quick-start/) 

## 安装docker

immich运行在docker上,因此在进行任何操作前必须安装docker

👉[Docker Desktop]([Docker Desktop: The #1 Containerization Tool for Developers | Docker](https://www.docker.com/products/docker-desktop/)) 你可以很轻松的下载到合适版本的docker

## 部署immich

docker安装完成后就该部署immich了

[这里](immich.app) 是immich的官方网站,[这里](https://github.com/immich-app/immich/releases/) 是他们的github页面

我们需要在他们的github页面上获取以下四个文件

docker-compose.yml (**核心服务定义**,定义了 Immich 运行所需的所有 Docker 服务（如 `server`、`web`、`microservices`、`Redis`、`Postgres` 等），以及它们之间的网络和卷配置。)

example.env (**环境变量**,包含部署所需的所有关键设置，例如数据库凭证、时区、密钥 (`SECRET`) 和上传目录路径等。您需要复制并修改它为.env)

hwaccel.ml.yml (**机器学习配置**一个 **Docker Compose 扩展文件**，用于配置机器学习服务（用于图像识别和搜索），使其能够利用硬件加速（如 GPU）。)

hwaccel.transcoding.yml (**硬件转码配置**,一个 **Docker Compose 扩展文件**，专门用于启用视频转码的硬件加速功能（如 Intel Quick Sync、Nvidia）。它会修改核心服务定义以暴露必要的设备。)

全部准备完成后将他们放在同一个目录下

`docker compose up -d`

即可自动的下载镜像并部署

需要更新的话

`docker compose pull`

就可以自动拉取新的镜像,并更新了

更新完成后,先前的镜像不会被删除,需要自己去docker desktop删除

## 中国大陆地区镜像被墙的解决方法

目前最好的方法就是给powershell配置一个http代理

   $Env:http_proxy="http:/ /127.0.0.1:你代理软件的监听端口";

   $Env:https_proxy="http:/ /127.0.0.1:你代理软件的监听端口";

这里为了防止被识别成链接打了两个空格,**请勿直接复制使用**



## 外部图库的使用方法

想要把自己的图片导入immich?使用外部图库可以轻松的导入大量的照片

打开 docker-compose.yml

![](https://cdn.jsdelivr.net/gh/hachiroku-lose/hachiroku-lose.github.io@image/blog-image/Snipaste_2025-11-21_16-46-05.png)

找到这个字段,

以 "E:/photo:/mnt/photo:rw" 这行举例子

`E:/photo `是你的外部图片所在的路径

`/mnt/photo`是你将`E:/photo` 映射到immich时的路径,一般会映射到`/mnt/xxx` ([Linux 系统目录结构]([Linux 系统目录结构 | 菜鸟教程](https://www.runoob.com/linux/linux-system-contents.html)))

`:rw` 这个路径是可 读(r) 写(w)的

然后打开你immich的webui页面,打开系统管理,找到外部图库,创建图库,选择图库所有者

![](https://cdn.jsdelivr.net/gh/hachiroku-lose/hachiroku-lose.github.io@image/blog-image/20251121181353082.png)

在这里填入你刚才**映射到immich内部的路径**

然后等待即可

## 配置公网访问

我个人比较喜欢使用  [Cloudflare Tunnel]([Cloudflare Tunnel · Cloudflare One docs](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/)) 来实现公网访问

操作方法如下

我不推荐直接安装他们提供的Windows的安装方法,因为会出现奇奇怪怪的问题

我个人更喜欢在docker下运行

新建一个空文件夹,这里我就叫它cloudflared好了

新建一个 docker-compose.yml 文件

version: "3"

services:

  cftunnel:

    image: cloudflare/cloudflared:latest

    container_name: cftunnel

    restart: unless-stopped

    command: tunnel --no-autoupdate run --token 你的token

然后 powershell运行

`docker compose up -d`

## 补充

修改immich任何的配置文件后,都需要重新运行

`docker compose up -d` 才能生效
