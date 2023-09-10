---
layout: post
title:  "Openwrt安装Home Assistant"
date:   2023-09-02 06:11:58 +0800
categories: Home Assistant
---


#### 安装主要步骤

1、安装dockerman

    #opkg install luci-app-dockerman

2、拉取homeassistant 稳定镜像

    # docker pull homeassistant/home-assistant:stable

3、创建网络

    docker network create -d bridge --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o "com.docker.network.bridge.name"="br-lan" lanet

4、创建容器

    docker run -d --restart=always --network lanet --ip=192.168.1.*(和网关ip不同即可） -v /root/ha/config:/config aarch64-homeassistant:stable
