---
layout: post
title:  "Openwrt Nginx配置"
date:   2024-3-16 06:11:58 +0800
categories: Openwrt Nginx
---

家里有群晖、HomeAssistant、JellyFin等多个外部服务，出现证书部署等管理方便的不便。由于原方案时在网关Openwrt上设置端口映射，导致内部服务无法对客户进行审查。于是计划把所有服务用Nginx统一管理，提高证书、安全策略等管理便捷性。

### 方案借鉴

在作为家庭网关的Openwrt路由器上，安装部署Nginx服务端。将内部的https服务改为http服务，使用Nginx反向代理转为https，实现证书统一管理。设置Nginx支持客户端IP传递，使得服务端可以支持客户端审计。

### 具体实施

#### Openwrt安装Nginx

笔者为了简便，直接使用Openwrt原生Nginx。如原服务不是Nginx，可以直接在包管理安装Nginx，包括luci-nginx。等待安装完成后，Nginx可自动接管uhttp实现Luci的Nginx管理。

#### Nginx配置

openwrt nginx依然使用uci配置，无法像标准Nginx那样直接使用nginx.conf配置。nginx配置文件由uci配置+模板自动生成。Nginx配置文件主要分为http配置、server配置、location配置，在openwrt 配置分别是：

1. http配置在配置模板中，位于/etc/nginx/uci.conf.template

2. server配置在uci中，可以直接修改/etc/config/nginx，或者使用UCI命令

3. location配置默认位于/etc/nginx/conf.d 目录下
