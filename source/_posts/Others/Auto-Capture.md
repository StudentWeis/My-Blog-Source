---
title: 电脑定时自动截屏
date: 2021-12-10 11:01:23
categories: 
        - 其他
tags: Quicker
---

每天都要在电脑前工作好久，总觉得时光流逝的悄无声息，就想记录些什么来试着留住时光。思来想去，最好的方式就是在电脑上设置定时自动截屏，记录每天的工作，这样一来，晚上回顾的时候也有些回忆可以追溯。在网上查找一番之后没有找到合适的教程，于是就自己设计了一套方案。

<!-- more -->

分析任务需求：

- 命令行全屏截图保存至指定文件；
- 按照截图的时间，对文件进行命名处理；
- 设置电脑开机之后定时自动运行；

使用到的软件：

- Snipaste
- Quicker

Snipaste 拥有命令行截图的功能，搭配 Quicker 的动作设计以及定时运行动作的功能可以很好地完成电脑定时截图存储功能。

### 1. Snipaste 命令行截图

[Snipaste](https://www.snipaste.com/) 是一款很好用的截屏软件，平常我经常使用它的贴图功能。它还有一个非常强大的功能：**命令行截图**。查阅 Snipaste 的帮助可知，不同的操作系统调用截图的命令不一致，以复制全屏截图为例：

* Windows 桌面版：`X:/path/to/your/Snipaste.exe snip --full -o clipboard`
* 微软商店版：`Snipaste snip --full -o clipboard`
* Mac 版：`/Applications/Snipaste.app/Contents/MacOS/Snipaste snip --full -o clipboard` 

上述方法会截取全屏图片，然后保存在剪切板中。不太适合我们的方案，作为日志来说，我们希望自动保存在一个日志文件夹中。

查阅帮助可知，需要修改选项为：`-o FILE_NAME`，其中 `FILE_NAME` 为文件路径。比如说，如果我们希望将自动截图保存到 `F:\TEMP\PicLog` 中，命名为 `2021-12-10.png`  图片文件，那么 `FILE_NAME` 应该为 `F:\TEMP\PicLog\2021-12-10.png`。

那么截图这边的工作就搞定了，接下来前往 Quicker 中进行动作设计。

### 2. Quicker 动作设计

[Quicker](https://getquicker.net/) 是一款非常好用的 Windows 下小工具合集软件，不过它最强大的功能在于可以让用户使用基本的模块搭建属于自己的工具。这里我自己搭建了一个小动作，来获取系统当前时间，并调用 Snipaste 命令行截图，保存至制定文件。

![Quicker动作](https://s2.loli.net/2021/12/10/mxKUYyi8Zl6JPn4.jpg)

设计非常简单：

1. 获取系统当前时间，将年月日、时分秒分别保存在不同的变量中；
2. 使用“组合成文本”模块将特定的命令行按照一定格式排布；
3. 运行上一步组合出的文本；

![Quicker组合成文本](https://s2.loli.net/2021/12/10/B2wx8jZMgP9nfiD.jpg)

Quicker 动作到此就设计完成了，大家如果不想自己设计的话也可以直接使用我分享的动作——[自动截图Snipaste](https://getquicker.net/Sharedaction?code=34e4b68a-33c4-4ebc-5453-08d9ba5900cc)。如果喜欢的话可以点个赞呀~有什么问题也可以反馈给我。

### 3. Quicker 定时自动运行

最后是最重要的一步，定时自动运行。这个就要使用 Quicker 的另一个功能了，自动运行动作，

![Quicker自动运行](https://s2.loli.net/2021/12/10/letvDoEmCY9AHgS.jpg)

非常简单，点开 Quicker 设置⚙ >> 辅助功能 >> 自动运行动作 >> 添加”定时运行“即可，可以自由设定时间。这里我设置的是每 10 分钟执行一次，这样就可以完成功能啦！需要注意的是，这个功能是需要专业版才可以用的。不过这样一款良心的软件，一年只要不到 60 块，我觉得还是值得的。填邀请码还附赠 90 天。

![Quicker定时任务](https://s2.loli.net/2021/12/10/Gg7ekMPbvoRJuq1.jpg)

### 4. 效果测试

如下所示👇，感觉还不错哈哈哈！

![效果测试](https://s2.loli.net/2021/12/10/EgIiLTu7O4GactY.jpg)
