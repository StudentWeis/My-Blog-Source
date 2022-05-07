---
title: Debian 11 蓝牙耳机 | NVIDIA 显卡游戏
date: 2022-05-07 11:27:17
tags: Linux
categories: Linux学习
---

前一阵子我把旧的 Lenovo ideapad 320S 换成了 Debian 11，想偶尔玩玩 Steam 游戏，看看视频、电影。使用过程中发现了一些问题：

- 不支持蓝牙协议 SBC，无法连接蓝牙耳机；
- Steam 游戏没使用 NVIDIA 的显卡，只用了 Intel 核显，出现明显卡顿；

经过一番努力，终于搞定，写下本文，供需要的朋友参考。

<!-- more -->

## 蓝牙耳机

买了小米的 Redmi Buds 3，蓝牙 5.2，采用 SBC 音频编码。Windows 是否认支持这种编码的，但 Debian 需要额外安装一些[驱动和配置](https://www.codercto.com/a/74283.html)。

安装相关软件包：

```sh
sudo apt install sbc-tools libsbc-dev libsbc1 bluez-firmware pulseaudio pulseaudio-module-bluetooth pavucontrol
```

将  `/etc/pulse/default.pa` 中的 `load-module module-bluetooth-discover` 注释。

并在 `/usr/bin/start-pulseaudio-x11` 中最后一个 `fi` 前加：

```sh
/usr/bin/pactl load-module module-bluetooth-discover
```

重启服务：

```sh
service bluetooth restart
sudo pkill pulseaudio
```

如不成功，重启电脑，执行以下命令加载模块

```sh
pactl load-module module-bluetooth-discover
```

## Steam 游戏使用 NVIDIA 显卡

Lenovo ideapad 320S 有一个 Intel 核显和一个 NVIDIA 独显，但在实际使用中我发现并没有用到 NVIDIA 独显。

- 安装 NVIDIA 显卡驱动；
- 使用 Steam 的 Launch Options 使能显卡；

安装显卡驱动：

```sh
# 需要最新的 gcc 以及 non-free 源

# 检测需要的驱动版本
sudo apt install nvidia-detect
nvidia-detect

# 安装对应的驱动
sudo apt install nvidia-driver

# 重启
sudo reboot

# 查看显卡运行状态
nvidia-smi
```

对于使用 Steam 的 Launch Options 使能显卡，Debian [官方 Wiki](https://wiki.debian.org/NVIDIA%20Optimus#Using_NVIDIA_PRIME_Render_Offload) 中有这样的说明：

Steam  games can be launched on your NVIDIA GPU by right-clicking their entry  to open the context menu, opening the "Properties" panel, clicking the  "Set Launch Options" button in the window that appears, and then setting the contents of the resulting text field to be: 

即在 Steam 中右键游戏，点击“属性”，点击“设置启动选项”，输入这一段指令即可：

```
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia %command%
```
