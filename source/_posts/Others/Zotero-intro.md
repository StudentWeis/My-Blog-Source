---
title: Zotero 简明介绍
date: 2021-11-16 22:52:23
categories: 
        - 其他
tags: 
        - 科研
        - Zotero
---

Zotero 是一个**开源**、易用的多平台科研工具，可用来帮助收集、组织、引用、分享各类资料，注册账号之后可以使用官方服务器在多端**自动同步**以及使用浏览器插件，还可以直接在网页中操作。

<!-- more -->

{% note info no-icon %}

持续更新中！

{% endnote %}

### 1. 简介

- 官网：https://www.zotero.org
- Github：https://github.com/zotero/zotero
- 下载安装：官网免费下载，安装按默认提示即可。

![Zotero概览](https://i.loli.net/2021/11/16/Qwv9RE3nW6FpbYZ.png)

### 2. 工作流

我用 Zotero 来完成学习资料组织的功能，主要的工作流是：收集资料 >> 组织整理收集到的资料 >> 学习并做记录 >> 导出数据 。另外由于有多台设备 ，需要多平台同步；可能也需要与研究伙伴分享一些资料。

#### 1）收集资料

Zotero 可以用来收集各种各样的资料文件，包括网页、PDF、图片、视频及电子邮件等。并且提供了多种方式来完成收集，包括浏览器插件添加、标识符（英：Identifier）添加、手动添加等。文件可以以两种方式储存：副本和链接。**副本**指的是会把添加的文件拷贝一份存储到 Zotero 的制定路径中统一管理；**链接**指的是不拷贝文件，文件由用户自己管理，Zotero 只是创建文件的快捷方式来管理。

我更倾向使用**链接**保存文件，因为这样可以更好地对所有资料进行管理，而不是任由 Zotero 分配。对于使用链接的情况，下面是我对各类资料的收集策略👇：

- **网页**：Zotero 默认收集网页的方式是**快照**（**Snapshop**），即将原网页的 HTML 及 CCS、JS 保存到本地，以备之后学习。这个方式的方便之处就是不用担心原网页丢失等情况，在断网情况下同样可以打开。使用浏览器插件直接可以一键选择分类添加。
- **PDF**：我倾向于把所有的文献 PDF 储存在一个文件中，然后通过坚果云在多平台之间同步，然后在 Zotero 中以**链接**的形式组织文件。这样的好处是我可以不受 Zotero 的限制，通过多种方式下载文献。在 Zotero 中选择“新建条目”，然后选择“文件链接”，找到对应文件添加即可。
- 其他文件同理。

对于收集到的资料，Zotero 提供了元数据提取的功能。比如说针对一篇文献，Zotero 可以很方便地提取作者、摘要、DOI、URL 等元数据，方便后续更好地整理和学习。Zotero 可以根据文件内容自动提取元数据，也可以后续手动添加或更改。添加问价之后，直接右键 >> ”Create Parent  Item“ >> 添加 DOI 即可。

> 一般来说英文文献的元数据提取成功率较高，中文文献往往都会失败，可以手动添加。

#### 2）组织整理

如果只是收集资料的话，就完全没有必要使用 Zotero 了，直接用资源管理器创建文件夹管理不香吗？所以使用 Zotero 最重要的就是它的组织管理功能。Zotero 主要支持两钟组织整理的方式：分类和标签（Collections and Tags）。

- **分类**：在左侧菜单中，可以创建分类，并可无限创建子分类。同一个文件可归属于多个分类中，这并不会拷贝文件，在不同分类中打开的都是同一份文件。所以可以灵活地使用分类功能，对同一份文件做多种归类，比如说：某来源、某课题、某任务等等。
- **标签**：在分类菜单的下方，是一个标签栏。可以为每一个文件添加标签，一个文件可以添加多个标签。可以为标签设置颜色，也可以设置位置，然后可以通过数字快捷键来快速为文件添加标签。设置了标签颜色之后，可以直接在文件列表中的文件前显示标签颜色的小方块提示，很方便。通常来说，可以设置一个 **To Read** 标签来标记那些没读过的资料，甚至可以使用 Emoji 做标签。

对于查找某文件，Zotero 提供了搜索、排序等功能。

#### 3）学习并记录

Zotero 提供了笔记功能来做学习笔记，框架基于 [TinyMCE](https://www.tiny.cloud/) HTML 编辑器，可以和 Markdown 很好地兼容，搭配 [ZotFile](http://zotfile.com/) 的 PDF 笔记提取功能可以很好地完成文献笔记的完成（后面会介绍）。笔记可以设置相关文件，也可以打标签。作为深度 Markdown 依赖者，我的笔记工作流是：浏览 PDF 的时候直接在 PDF 文件里做笔记，然后使用 ZotFile 提取出来，复制到对应的 Markdown 中进行整理，然后再复制回来以便之后的查看。

#### 4）导出数据

Zotero 对于 MS Word 等文档编辑软件有很好的支持，可以很方便的导出参考文献（Creating Bibliographies）到文档中，当然这需要一个精确的元数据。有多种引用样式可以选择，在设置中可以添加自定义引用样式，也可以添加某些官方样式，比如说中国国标，如下图所示：

![Zotero 文章引用样式](https://i.loli.net/2021/11/16/18wHRArWbuMzGLX.jpg)

而 Zotero 导出引用目录的方法非常简单：在文件列表中摁住 <kbd>Ctrl</kbd> 然后选择想要添加的文件，直接右键导出引文目录即可；也可以导出某一分类的所有文件，这个功能非常方便。

Zotero 可以很好地和 MS Word 联动起来，安装 Zotero 之后打开 Word，会看到顶部菜单栏中有 Zotero 的插件。

#### 5）多平台同步与分享

- **同步**：注册了账号之后可以直接在设置中关联账号，然后 Zotero 会使用专用的服务器进行自动同步。也可以使用自定义的 WebDAV 进行同步，进行简单的配置即可。
- **分享**：Zotero 也可以当作一个科研人员的社交平台，每个人都有自己的简介页面（[这](https://www.zotero.org/studentwei)是我的）。可以创建群组来共享分享列表。然后也可以新建订阅，来订阅别人的某一个分类，这样在别人更新之后就能同步进行更新。

---

### 3. 插件系统

Zotero 提供了插件系统，用户可以添加或者开发一些第三方插件来提升软件使用体验，可以在插件清单 [Plugins for Zotero](https://www.zotero.org/support/plugins) 寻找插件。安装插件的方法非常简单：在插件清单中寻找想要的插件，下载 `.xpi` 文件，然后打开 Zotero，点击顶部“工具” >> “插件” >> ⚙ >> “Install App-on From Files”，然后选择对应文件即可。

#### ZotFile

- Zotfile 是 Zotero 的一个用来管理附件的插件。
- 官网：http://zotfile.com、Github：https://github.com/jlegewie/zotfile

特色功能介绍：

- 提取 PDF 注释：右键对应的 PDF >> “Manage Attachments” >> “Extract Annotations”，稍等一下，即可将 PDF 中的注释提取出来，在到同条目下生成笔记。

### 参考资料

- 官网：[Collections and Tags](https://www.zotero.org/support/collections_and_tags)
- 官网：[Notes](https://www.zotero.org/support/notes)
- 官网：[Bibliographies](https://www.zotero.org/support/creating_bibliographies)
- 官网：[Syncing](https://www.zotero.org/support/sync)
- 知乎：[Word 联动](https://zhuanlan.zhihu.com/p/97855586)
- 知乎：[Zotero 生态](https://zhuanlan.zhihu.com/p/97855586)
- 博客：[Zotero 插件](https://iseex.github.io/2020-07/Zotero-tools-bag)

