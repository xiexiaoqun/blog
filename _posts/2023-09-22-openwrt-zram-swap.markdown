---
layout: post
title:  "Openwrt ZRAM Swap设置"
date:   2023-09-22 06:11:58 +0800
categories: Openwrt Zram Swap
---

### 背景

使用Docker后，感觉1G Ram对Openwrt来说，有点局促。通过Linux zram swap 用CPU资源换取一些可用RAM。 

### 依赖

首先要安装如下几个依赖的包

1. kmod-zram ——内核zram模块

2. zram-swap ——zram 配置脚本

### 配置

笔者使用的openwrt 的系统菜单的“系统”选项中有“ZRam设置”，可以配置ZRam的大小和算法。也可以配置uci 的 system.@system[0].zram_size_mb 和system.@system[0].zram_comp_algo 两个选项来实现。**重启后生效**。

**注意**：zram默认支持的算法是lzo，如果要换成诸如lz4算法，需要安装kmod-lib-lz4等算法库，使得内核支持这些算法。 
