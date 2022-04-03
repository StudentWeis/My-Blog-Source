---
title: 在Linux中检索单词,字母q总是与u在一起？
date: 2021-03-20 12:56:17
tags: Linux
categories: Linux学习
---

前些天在学英语的时候遇到了一个有趣的说法：“字母q总是与u在一起”，一时求证的欲望大起，找到了一个用 Linux 来操作的方法，在此记录一下。

<!-- more -->

## 操作步骤

### 1、思路

首先要明确自己的想法：得找到一个相对来说比较全的字典，而且方便查询，可以查到所有拼写中带有“q”的单词，接下来再在其中删去“qu”连在一起的单词，显示出来。

### 2、工具

#### 字典

系统：Ubuntu 18.04 

在目录`/usr/share/dict/`中有一个`words`文件，里面存储了九万多个英语单词，常用于系统对于单词拼写的报错。相对来说是一个相当全的词汇集合了，因此我们使用这个文件来进行查询。

#### 指令

具体用到的指令如下：

`grep`：在某一文件中查询某一字符串，==当没有输入文件名时，默认在当前的工作区间内查找==。

`wc`：统计某一文本的数量。

### 3、操作

1. 由于`words`文件中含有大量的以`'s`结尾的所有格形式，所以第一步就是删去这些词语。

```shell
grep "'s$" /usr/share/dict/words -v |

# 输入|的目的是为了继续输入后面的指令。
# $是每行单词的结束符
# -v 参数是“删去”
```

2. 接下来是搜索所有带有`q`的单词。

```shell
grep "q" -i |

# -i 参数是“忽略大小写的区别”
```

3. 接下来是删除所有带有`qu`的单词。

```shell
grep "qu" -i -v 
```

然后回车，搞定，屏幕上已经显示出来了所有带有`q`的单词中qu没连在一起的单词！

结果如下图所示：

<img src="https://img-blog.csdnimg.cn/20210313162107631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTk0MzU4Nw==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:80%;" />

可以明显地看到，这些单词只是一些生僻的人名和地名，而且也只有短短二十几个。

那么含有 q 的单词有多少个呢？

我们用`wc`指令来查看一下

4. 查看含有 q 的单词数量。

```shell
grep "'s$" /usr/share/dict/words -v | grep "q" -i | wc -l

# -l 参数是“统计行数”
```

结果如下图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210313190339162.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTk0MzU4Nw==,size_16,color_FFFFFF,t_70)

我们可以看到，含有 q 的英语单词有 1143 个，但是 qu 不连在一起的情况却只有 20 几个，而且还是生僻词。看来“字母q总是与u在一起吗？”的答案是肯定的啦！



-----

5. 在做完一步之后，我们可以打印几个含有`q`的单词看一看。

```shell
grep "q" /usr/share/dict/words -i
```

结果如下图所示：

<img src="https://img-blog.csdnimg.cn/20210313162200478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTk0MzU4Nw==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:80%;" />



## 结语

啧啧啧，没想到还真是这样，太有趣了。

自己试着分析了一下，觉得这可能是由于发音的原因！

好了，不多说了，俺继续去学英语了。

参考：https://www.douban.com/note/149809926/