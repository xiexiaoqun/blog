---
layout: post
title:  "Proxmox（PVE）常见配置分享"
date:   2024-6-26 08:11:58 +0800
categories: PVE
---

最近更换了PVE虚拟化平台，替代商用的ESXi。笔者使用的是PVE 8.2，Kernel 6.8。

### 更换PVE原因

1. Vmware卖给博通后，License不再对家庭或个人用户免费；

2. 支持网站变更的不友好（笔者花了很久都没找到patch在哪里下载）；

3. 家用的硬件平台不支持IPMI，ESXi缺乏对硬件监控等功能。

### PVE安装

Linux 发行版本安装难度类似，不做详细说明。

### 常用配置

#### 软件源配置

跟Debian系统相比，有一些差异。主要差异为：

1. 多了proxmox的pve源配置和ceph配置

2. pve源社区用户可以设置为pve-no-subscription

#### 屏蔽订阅提醒

修改js文件：**/usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js**

搜索关键词：**No valid subscription**

删除该弹出代码即可。

系统如果更新proxmox 包，有可能需要重新修改。

#### 监控硬件温度

可以使用Debian自带的sensors来查看硬件温度。安装命令：

```
# apt-get install lm-sensors
```

查看硬件温度命令：

`# sensors`

#### 硬件直通

笔者使用的是PVE 8.2，Kernel 6.8，并不需要按照手册介绍的开启iommu和vfio，内核已经默认开启了这些功能，可以直接使用。
