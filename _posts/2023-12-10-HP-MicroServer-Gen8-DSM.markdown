---
layout: post
title:  "HP Micro Server Gen8 黑群晖安装"
date:   2023-12-10 06:11:58 +0800
categories: HP 群晖
---

HP Micro Server Gen8是一款比较老的设备，对群晖支持比较友好。本文以7.2.1版本为例。

### 过程简述

HP Micro Server Gen 8 主板自带被识别为USB设备的SD卡槽，相比其他机器可以选择不使用U盘来引导群晖启动，使用起来比较方便，但是安装的时候存在一定差异。以下以该方案为例。

主要步骤：

1.  设置服务器第一启动设备为usb设备
2.  使用U盘启动linux live版本，将tinycore-redpill写入sd卡
3.  在tinycore-redpill中编译引导
4.  启动群晖

### 具体步骤

#### 设置服务器第一启动设备为usb设备

直接在iLO中设置即可，也是有iLO方便之处。设置BIOS，硬盘启动模式修改为ACHI模式，不要使用raid卡模式

#### 使用U盘启动linux live版本，将tinycore-redpill写入sd卡

此处推荐Puppy Linux([Puppy Linux Home (puppylinux-woof-ce.github.io)](https://puppylinux-woof-ce.github.io/))，笔者测试了Ubuntu等其他系统的live usb启动，在远程启动后因为兼容性问题，性能较差。

使用dd命令将tinycore-redpill img（[pocopico/tinycore-redpill (github.com)](https://github.com/pocopico/tinycore-redpill)）文件直接写在sd卡中即可。也可以把sd卡取出来，用其他PC来写，这种方法需要拆机，略麻烦。

完成tinycore-redpill写入后，不再需要U盘，可以拔出

#### 在tinycore-redpill中编译引导

在tinycore的terminal中执行如下命令编译：

./rploader.sh serialgen  DS3622xs+

./rploader.sh identifyusb

./rploader.sh satamap

./rploader.sh build ds3622xsp-7.2.1-69057

遇到需要确认的，一律y

#### 启动群晖

启动后，在引导界面可以看到群晖的IP地址（也可在路由器DHCP列表里看到），浏览器访问改IP地址后，上传在群晖官网，下载ds3622xsp的7.2.1 pat文件，即可。

### 可能遇到的问题

#### rploader.sh build 命令提示没有相应的版本，

可以修改/home/tc/custom\_config.json文件，按照相近的如ds3622xsp-7.2.0，添加ds3622xsp-7.2.1-69057编译模板

#### 老版本的群晖升级

可以直接编译其他版本的群晖引导。启动引导后，系统会提示硬盘迁移，可以无损更换到新系统，即使设备或者版本发生更换。**注意：数据备份时必须的，防止意外发生。**





