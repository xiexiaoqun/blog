---
layout: post
title:  "Openwrt IPTV转发内网"
date:   2023-09-11 06:11:58 +0800
categories: Openwrt IPTV
---

### Openwrt转发IPTV

Openwrt转发IPTV网上有两种方式：

1. 将IPTV 组播直接转发至家庭内网

2. 将IPTV转为单播Http媒体至内网

第一种方式需要使用omcproxy，暂时没尝试。

第二种方式需要使用udpxy或msd_lite，本文主要分享的方法

### 依赖的包安装

以msd_lite为例，可以直接安装luci-app-msd_lite，关联依赖msd_lite自动安装。可在luci中直接安装，或使用命令行安装，例如：

```
#opkg install luci-app-msd_lite
```

### 网络配置

本人使用的IPTV使用PPPoE拨号方式来进入IPTV网络，有的地方使用静态IP或者DHCP绑定MAC等方式，可灵活选择方式。

获取具体上网方式的办法，可以在运营商的IPTV盒子查看具体上网方式，也可以联系运营商获取PPPoE上网账号等信息。**PPPoE连接务必配置不作为默认路由，会导致网络无法上网。**

### 获取IPTV组播地址

1. 可以直接在网上搜索到本地运营商IPTV组播地址m3u文件，根据本地网络配置，稍作修改即可。

2. 抓包方式可以参考其他网上教程，此处不在多言。

### 配置msd_lite

使用Luci-app-msd_lite默认设置即可，老版本msd_lite存在bug，使用最新版本没有任何问题。

### 问题及解决

**防火墙配置**

如果Interface显示有数据流量，而客户端(msd_lite或udpxy)无法读取到数据，需要考虑在防火墙规则中，添加组播地址udp报文的放行规则。如果可以正常使用，则可以不做配置。

**无法收到组播数据**

- 笔者所在运营商使用了比较老的IGMPv2（抓包得知），openwrt最新版本使用的IGMPv3，需要配置系统IGMP版本。参考Openwrt手册，在/etc/sysctl.conf文件追加配置：

```
net.ipv4.conf.all.force_igmp_version=2
```

- 如果交换机支持IGMP Snooping，可能出现不兼容问题，导致无法收到IGMP报文。家庭网络IGMP消息不多，对IGMP Snooping功能不敏感，可以尝试关闭交换机上的IGMP Snooping。
