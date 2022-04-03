---
title: 解决STM32H750-USB虚拟串口无法被识别
date: 2021-12-17 22:27:41
tags: 
		- 单片机
		- STM32
		- CubeMX
categories: 
        - 单片机
---

开发 STM32 的时候，需要串口、调试器等等外接设备来方便调试，可是对于我来说，那一大堆的线实在不够优雅。而通过 USB-DFU 下载程序、USB 虚拟串口打印信息，则只要一根 USB 即可搞定，非常简洁。

可是今天在用 CubeMX 开发 STM32H750VBT 的时候，USB 总是**无法被识别**。经过了一阵的摸索，终于找到了问题的原因——时钟配置。在此记录一下。

<!-- more -->

首先我先在网上查找了相关的资料，一般来说，STM32 USB 虚拟串口无法被识别可能有两种情况：

- 堆栈设置太小，USB 无法完成初始化，在 CubeMX 上设置大一点。
- 电脑端使用了 USB 分线器，把 USB 直接连接到电脑上。

两种方法我都尝试了，无效。然后我就开始思考，是不是因为 STM32H750 的时钟主频太高了（400MHz），导致时钟出错？然后我就把主频设置为了 100MHz，欸？结果可以了，终于被识别到了。但是把主频设置为 200MHz 就又不行了。可是我用 H7 不就是为了高主频吗？低主频还有什么意义？

进一步我猜想，USB 外设时钟有一个最高限速，说不定只要把 USB 外设时钟限制好就可以了。果真，在网上浏览相关信息的时候看到一篇[文章](https://www.waveshare.net/study/article-664-1.html)，提到了 USB 时钟频率应该设置为 48MHz，我也才知道了原来 CubeMX 中 STM32H750 还能设置 USB 外设的时钟。

---

在 Clock Configuration 中配置 USB Clock Mux 为 RC48👇，可以获得精确的 48MHz，否则在其他频率时 USB 初始化会发生错误。

![STM32H7-USB时钟问题](https://s2.loli.net/2021/12/17/atWkwGL8JcUmgxh.jpg)

这一点在《STM32H750 Reference Manual》中 P2619 也有说明👇，即 USB OTG 的接收到的时钟应为 48MHz。

![STM32H7-USB时钟问题手册说明](https://s2.loli.net/2021/12/17/l8hPzQerMf1CDdY.jpg)

