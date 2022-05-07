---
title: Lenovo ideapad 320S 重装 Debian 11
date: 2022-04-20 12:56:17
tags: Linux
categories: Linux学习
---

买了新的笔记本，旧的本子想换个 Linux 再多撑几年。定位就是偶尔玩玩 Steam 上的小游戏，看看视频、电影什么的。型号为 Lenovo ideapad 320S，两块硬盘，一块固态一块机械，原有 Windows 10 不保留，直接整个重装。安装 Debian 11 至固态硬盘，图形界面选择 KDE Plasma。

<!-- more -->

## 安装流程

1. 下载 Debian 11 镜像，并烧录至 U 盘中。
2. 关闭 Intel RST Mode。
3. 插 U 盘安装 Debian 11。
4. 准备好网卡驱动 firmware-athok，U 盘安装。
5. 连接 WiFi。
6. 选择图形界面 KDE，开始安装。
7. 安装 Steam。

## 细节

### 1. Intel RST Mode

如果不关闭这个模式，则 Debian 无法识别固态硬盘。需提前进入 BIOS 关闭 Intel RST Mode。

### 2. 网卡驱动

Debian 原生不支持非自由的闭源驱动，需要手动下载之后通过 U 盘安装。

下载 [firmware-atheros](https://packages.debian.org/bullseye/all/firmware-atheros)，解压，取出 athk10 文件夹，存进 U 盘中，插在笔记本上即可。

ath10k/pre-cal-pci-0000:01:00.0.bin 等[无关紧要](https://forums.debian.net/viewtopic.php?p=703617)。

### 3. Steam

```sh
# 由于许多游戏支持 32 位架构，因此我们需要在安装 Steam 之前在 Debian 上启用其 32 位支持
sudo dpkg --add-architecture i386

# 添加 non-free 源
sudo vim /etc/apt/sources.list

# deb http://deb.debian.org/debian/ bullseye main non-free contrib
# deb http://security.debian.org/debian-security bullseye/updates main contrib non-free

sudo apt update
sudo apt install steam
```

## 效果

系统内存占用只有 0.5 GB 左右，对比原来 Windows 10 的 3.5 GB，真的不要再流畅。原来的系统刚开机风扇就呼呼地转，现在的系统即使打开浏览器看视频、打开 Steam 风扇都不怎么转，温度非常低。这次重装系统真的是非常成功了！



