---
title: 博客搭建记录-持续更新
date: 2022-03-15 20:07:33
categories: 
        - 博客日志
---

一直觉得可以在 Digital World 中拥有一个自己的小天地是很开心的一件事。2021 年春天，我终于搭建了自己的博客，基于 Hexo 框架。需要的基础服务有 Node.js、Git、Hexo。最好都用最新版的，这样就可以跟上官方的节奏，使用最新的特性，文档、社区各种方便。而且更新的时候也会省事很多。迁移的时候直接迅速搬运重搭就可以。本文为笔者的 Hexo 博客搭建记录，使用国人制作的优雅的 NexT 主题，将实时跟随 Hexo、NexT 的最新版本进行更新。

<!-- more -->

目前版本：

- **Hexo**：6.1.0
- **NexT**：8.10.1

相关术语介绍：

- **Hexo**：快速搭建博客的一个框架。
- **Gitee Page**：是一个支持 Jekyll、Hugo、Hexo 的静态网站的服务。
- **Next**：Hexo 中的一个非常好用、简洁的主题。
- **Node.js**：Hexo 所用到的 JS 服务端框架。
- **npm**：JS 包管理器，用于下载 Hexo 及其插件。
- **Git**：版本管理工具，在 Hexo 中用于博客内容部署。

{% note info %}

**提示**

本文还在更新中！请持续跟踪！

本文仅为笔者简单记录，有些内容不够详细，有不懂的地方可以发邮件问我：

studentweis@gmail.com

目前还差：自定义博客配置文件详细解读。

{% endnote %}

## 一、环境配置

### 1. 安装 Node.js、npm

Node.js 在官网下载最新的然后安装就好，其中默认含有 npm。

[Node.js 官网](https://nodejs.org)、[npm 官网](https://www.npmjs.com)

```shell
# 安装完之后查看版本
npm -v
node -v
```

### 2. 安装 Git

在官网下载最新的然后安装就好。

[Git 官网](https://git-scm.com)

```shell
# 安装完之后查看版本
git --version
```

### 3. 安装 Hexo

[Hexo 官网](https://hexo.io/)

```shell
# 使用 npm 直接下载最新版
npm install hexo

# 如果无法下载最新版可以直接指定下载的版本
npm install hexo@6.1.0
```

## 二、建站

### 1. 配置 Hexo

参考[官网文档](https://hexo.io/zh-cn/docs/setup)

```shell
hexo init <文件夹路径>
```

然后即可在本地中浏览 Hexo 博客初始内容：

```shell
hexo server
```

### 2. 安装 NexT

参考 [NexT 官网](https://theme-next.js.org)

```shell
# 在博客的根目录下
git clone https://github.com/next-theme/hexo-theme-next themes/next
```

```yaml
# 在 Hexo __config.yml 中修改主题
theme: next
```

然后可先在本地中浏览一下博客：

```shell
hexo server
```

### 3. Gitee 建站

新建一个仓库，选择公开，仓库名字选择与 Gitee 账户同名。[关于 Gitee 仓库命名的官方解释](https://gitee.com/help/articles/4136#article-header0)

修改 Hexo 配置，选择部署：

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: 'git'
  repo: https://gitee.com/username/username.git # 仓库名字要与 Gitee 账户名相同
  branch: main
```

之后在 Gitee 项目主页上选择“服务” >> Gitee Pages，然后完成认证即可。

然后输入下列命令：

```shell
npm install hexo-deployer-git --save
hexo g --d  #一键部署
```

部署完成之后需要去 Gitee Pages 点击更新。

点击 “强制 HTTPS” 可以使博客拥有 SSL 安全证书，最好点上。

### 4. 腾讯云服务器建站

这个涉及到了一些更复杂的操作，之后我会专门写一篇博客来记录这个过程。

本博客目前就是部署于腾讯云服务器上，使用了我自己独立的域名。

## 三、自定义博客配置文件

Hexo、NexT 博客有两个配置文件：Hexo 的 `__config.yml` 和 NexT 的 `__config.yml`，分别用作 Hexo 基本配置和 Next 主题配置。制作博客时可以按照配置文件进行配置和学习，同时参考官方文档。

### 1. Hexo 基本配置

编辑 Hexo 根目录下的 `__config.yml` 文件。

博客名字、描述、作者、博客所用语言。

```yaml
title: 山冰之冰 # 博客名
subtitle: ''
description: '逝者如斯夫，不舍昼夜'	# 博客描述
keywords:
author: StudentWei	# 博客作者
language: zh-CN		# 博客语言
timezone: ''		# 博客时区，默认为电脑配置
```

### 2. Next 配置文件设置

编辑 themes/next 下的 `__config.yml` 文件。

#### 网站信息配置

设置网站图标、知识共享协议

要注意简体中文的 language 选择为：`deed.zh`

image 文件夹在：`\themes\next\source\images` 中。

```yaml
# ---------------------------------------------------------------
# Site Information Settings
# ---------------------------------------------------------------

favicon:
  small: /images/favicon-16x16-next.png
  medium: /images/favicon-32x32-next.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  #safari_pinned_tab: /images/logo.svg
  #android_manifest: /manifest.json

# Custom Logo (Warning: Do not support scheme Mist)
custom_logo: #/uploads/custom-logo.jpg

# Creative Commons 4.0 International License.
# See: https://creativecommons.org/about/cclicenses/
creative_commons:
  # Available values: by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | cc-zero
  license: by-nc-sa
  # Available values: big | small
  size: small
  sidebar: true
  post: true
  # You can set a language value if you prefer a translated version of CC license, e.g. deed.zh
  # CC licenses are available in 39 languages, you can find the specific and correct abbreviation you need on https://creativecommons.org
  language: deed.zh
```

#### 菜单配置

翻译配置文件，在 \themes\next\languages 中找到对应的语言翻译配置文件，可以自己添加。

```yaml
# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------

# Usage: `Key: /link/ || icon`
# Key is the name of menu item. If the translation for this item is available, the translated text will be loaded, otherwise the Key name will be used. Key is case-sensitive.
# Value before `||` delimiter is the target link, value after `||` delimiter is the name of Font Awesome icon.
# External url should start with http:// or https://
menu:
  home: / || fa fa-home
  about: /about/ || fa fa-user
  #tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat

# Enable / Disable menu icons / item badges.
menu_settings:
  icons: true
  badges: false
```

添加对应文件

参考：https://blog.csdn.net/weixin_33857230/article/details/91474562

1. 生成界面

```shell
hexo new page categories
hexo new page tags
hexo new page about
```

2. 添加类型

   type: categories

#### 侧边栏配置

```yaml
# ---------------------------------------------------------------
# Sidebar Settings
# See: https://theme-next.js.org/docs/theme-settings/sidebar
# ---------------------------------------------------------------

# Table of Contents in the Sidebar
# Front-matter variable (nonsupport wrap expand_all).
toc:
  enable: true
  # Automatically add list number to toc.
  number: false
  # If true, all words will placed on next lines if header width longer then sidebar width.
  wrap: false
  # If true, all level of TOC in a post will be displayed, rather than the activated part of it.
  expand_all: false
  # Maximum heading depth of generated toc.
  max_depth: 6
```

#### 侧边栏

```yaml
sidebar:
  # Sidebar Position.
  position: left
  #position: right

  # Manual define the sidebar width. If commented, will be default for:
  # Muse | Mist: 320
  # Pisces | Gemini: 240
  #width: 300

  # Sidebar Display (only for Muse | Mist), available values:
  #  - post    expand on posts automatically. Default.
  #  - always  expand for all pages automatically.
  #  - hide    expand only when click on the sidebar toggle icon.
  #  - remove  totally remove sidebar including sidebar toggle.
  display: always

  # Sidebar padding in pixels.
  padding: 18
  # Sidebar offset from top menubar in pixels (only for Pisces | Gemini).
  offset: 12
```

#### 头像

```yaml
# Sidebar Avatar
avatar:
  # Replace the default image and set the url here.
  url: #/images/avatar.gif
  # If true, the avatar will be displayed in circle.
  rounded: false
  # If true, the avatar will be rotated with the cursor.
  rotated: false
```

#### 网站状态

是否在侧边栏显示博客的文章、分类、标签

```yaml
# Posts / Categories / Tags in sidebar.
site_state: true
```

#### 其他网站地址

```yaml
# Social Links
# Usage: `Key: permalink || icon`
# Key is the link label showing to end users.
# Value before `||` delimiter is the target permalink, value after `||` delimiter is the name of Font Awesome icon.
social:
  #GitHub: https://github.com/yourname || fab fa-github
  #E-Mail: mailto:yourname@gmail.com || fa fa-envelope
  #Weibo: https://weibo.com/yourname || fab fa-weibo
  #Google: https://plus.google.com/yourname || fab fa-google
  #Twitter: https://twitter.com/yourname || fab fa-twitter
  #FB Page: https://www.facebook.com/yourname || fab fa-facebook
  #StackOverflow: https://stackoverflow.com/yourname || fab fa-stack-overflow
  #YouTube: https://youtube.com/yourname || fab fa-youtube
  #Instagram: https://instagram.com/yourname || fab fa-instagram
  #Skype: skype:yourname?call|chat || fab fa-skype

social_icons:
  enable: true
  icons_only: false
  transition: false
```

#### 友链传递

```yaml
# Blog rolls
links_settings:
  icon: fa fa-globe
  title: Links
  # Available values: block | inline
  layout: block

links:
  #Title: https://example.com
```

#### 目录

侧边栏目录自动标号：toc >> number

```yaml
# Table of Contents in the Sidebar
# Front-matter variable (nonsupport wrap expand_all).
toc:
  enable: true
  # Automatically add list number to toc.
  number: true
  # If true, all words will placed on next lines if header width longer then sidebar width.
  wrap: false
  # If true, all level of TOC in a post will be displayed, rather than the activated part of it.
  expand_all: false
  # Maximum heading depth of generated toc.
  max_depth: 6
```

#### 网页底部设置

```yaml
# ---------------------------------------------------------------
# Footer Settings
# See: https://theme-next.js.org/docs/theme-settings/footer
# ---------------------------------------------------------------

# Show multilingual switcher in footer.
language_switcher: false

footer:
  # Specify the year when the site was setup. If not defined, current year will be used.
  #since: 2021

  # Icon between year and copyright info.
  icon:
    # Icon name in Font Awesome. See: https://fontawesome.com/icons
    name: fa fa-heart
    # If you want to animate the icon, set it to true.
    animated: false
    # Change the color of icon, using Hex Code.
    color: "#ff0000"

  # If not defined, `author` from Hexo `_config.yml` will be used.
  copyright:

  # Powered by Hexo & NexT
  powered: true

  # Beian ICP and gongan information for Chinese users. See: https://beian.miit.gov.cn, http://www.beian.gov.cn
  beian:
    enable: false
    icp:
    # The digit in the num of gongan beian.
    gongan_id:
    # The full num of gongan beian.
    gongan_num:
    # The icon for gongan beian. See: http://www.beian.gov.cn/portal/download
    gongan_icon_url:
```

#### Post 设置

设置文章摘要、文章元数据显示、标签图标替换、打赏、Follow me、相关博客、博客编辑

- 显示摘要：官方提供了两种方式：It is recommended to use `<!-- more -->` (the first way) which can not only control what you want to show better, but also let Hexo's plugins use it easily.

```yaml
# ---------------------------------------------------------------
# Post Settings
# See: https://theme-next.js.org/docs/theme-settings/posts
# ---------------------------------------------------------------

# Automatically excerpt description in homepage as preamble text.
excerpt_description: true

# Read more button
# If true, the read more button will be displayed in excerpt section.
read_more_btn: true

# Post meta display settings
post_meta:
  item_text: true
  created_at: true
  updated_at:
    enable: true
    another_day: true
  categories: true

# Post wordcount display settings
# Dependencies: https://github.com/next-theme/hexo-word-counter
symbols_count_time:
  separated_meta: true
  item_text_total: false

# Use icon instead of the symbol # to indicate the tag at the bottom of the post
tag_icon: false

# Donate (Sponsor) settings
# Front-matter variable (nonsupport animation).
reward_settings:
  # If true, a donate button will be displayed in every article by default.
  enable: false
  animation: false
  #comment: Buy me a coffee

reward:
  #wechatpay: /images/wechatpay.png
  #alipay: /images/alipay.png
  #paypal: /images/paypal.png
  #bitcoin: /images/bitcoin.png

# Subscribe through Telegram Channel, Twitter, etc.
# Usage: `Key: permalink || icon` (Font Awesome)
follow_me:
  #Twitter: https://twitter.com/username || fab fa-twitter
  #Telegram: https://t.me/channel_name || fab fa-telegram
  #WeChat: /images/wechat_channel.jpg || fab fa-weixin
  #RSS: /atom.xml || fa fa-rss

# Related popular posts
# Dependencies: https://github.com/tea3/hexo-related-popular-posts
related_posts:
  enable: false
  title: # Custom header, leave empty to use the default one
  display_in_home: false
  params:
    maxCount: 5
    #PPMixingRate: 0.0
    #isDate: false
    #isImage: false
    #isExcerpt: false

# Post edit
# Easily browse and edit blog source code online.
post_edit:
  enable: false
  url: https://github.com/user-name/repo-name/tree/branch-name/subdirectory-name/ # Link for view source
  #url: https://github.com/user-name/repo-name/edit/branch-name/subdirectory-name/ # Link for fork & edit

# Show previous post and next post in post footer if exists
# Available values: left | right | false
post_navigation: left
```

#### 其他设置

顶部阅读进度条、阅读进度百分比、代码复制、书签

```yaml
# ---------------------------------------------------------------
# Misc Theme Settings
# See: https://theme-next.js.org/docs/theme-settings/miscellaneous
# ---------------------------------------------------------------

# Preconnect CDN for fonts and plugins.
# For more information: https://www.w3.org/TR/resource-hints/#preconnect
preconnect: false

# Set the text alignment in posts / pages.
text_align:
  # Available values: start | end | left | right | center | justify | justify-all | match-parent
  desktop: justify
  mobile: justify

# Reduce padding / margin indents on devices with narrow width.
mobile_layout_economy: false

# Browser header panel color.
theme_color:
  light: "#222"
  dark: "#222"

# Override browsers' default behavior.
body_scrollbar:
  # Place the scrollbar over the content.
  overlay: false
  # Present the scrollbar even if the content is not overflowing.
  stable: false

codeblock:
  # Code Highlight theme
  # All available themes: https://theme-next.js.org/highlight/
  theme:
    light: default
    dark: tomorrow-night
  prism:
    light: prism
    dark: prism-dark
  # Add copy button on codeblock
  copy_button:
    enable: false
    # Available values: default | flat | mac
    style:

back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: false
  # Scroll percent label in b2t button.
  scrollpercent: false

# Reading progress bar
reading_progress:
  enable: false
  # Available values: left | right
  start_at: left
  # Available values: top | bottom
  position: top
  reversed: false
  color: "#37c6c0"
  height: 3px

# Bookmark Support
bookmark:
  enable: false
  # Customize the color of the bookmark.
  color: "#222"
  # If auto, save the reading progress when closing the page or clicking the bookmark-icon.
  # If manual, only save it by clicking the bookmark-icon.
  save: auto

# `Follow me on GitHub` banner in the top-right corner.
github_banner:
  enable: false
  permalink: https://github.com/yourname
  title: Follow me on GitHub
```

#### 字体

注意 family 要选对。

```yaml
font:
  enable: true

  # Uri of fonts host, e.g. https://fonts.googleapis.com (Default).
  host: https://fonts.googleapis.com

  # Font options:
  # `external: true` will load this font family from `host` above.
  # `family: Times New Roman`. Without any quotes.
  # `size: x.x`. Use `em` as unit. Default: 1 (16px)

  # Global font settings used for all elements inside <body>.
  global:
    external: true
    family: Noto Serif SC
    size:

  # Font settings for site title (.site-title).
  title:
    external: true
    family:
    size:

  # Font settings for headlines (<h1> to <h6>).
  headings:
    external: true
    family:
    size:

  # Font settings for posts (.post-body).
  posts:
    external: true
    family:

  # Font settings for <code> and code blocks.
  codes:
    external: true
    family:
```

#### SEO 设置

https://theme-next.js.org/docs/theme-settings/seo.html

按照官网教程来就好。

```yaml
# ---------------------------------------------------------------
# SEO Settings
# See: https://theme-next.js.org/docs/theme-settings/seo
# ---------------------------------------------------------------

# If true, site-subtitle will be added to index page.
# Remember to set up your site-subtitle in Hexo `_config.yml` (e.g. subtitle: Subtitle)
index_with_subtitle: false

# Automatically add external URL with Base64 encrypt & decrypt.
exturl: false
# If true, an icon will be attached to each external URL
exturl_icon: true

# Google Webmaster tools verification.
# See: https://developers.google.com/search
google_site_verification:

# Bing Webmaster tools verification.
# See: https://www.bing.com/webmasters
bing_site_verification:

# Yandex Webmaster tools verification.
# See: https://webmaster.yandex.ru
yandex_site_verification:

# Baidu Webmaster tools verification.
# See: https://ziyuan.baidu.com/site
baidu_site_verification:
```

#### 动画设置

```yaml
# ---------------------------------------------------------------
# Animation Settings
# ---------------------------------------------------------------

# Use Animate.css to animate everything.
# For more information: https://animate.style
motion:
  enable: true
  async: false
  transition:
    # All available transition variants: https://theme-next.js.org/animate/
    post_block: fadeIn
    post_header: fadeInDown
    post_body: fadeInDown
    coll_header: fadeInLeft
    # Only for Pisces | Gemini.
    sidebar: fadeInUp

# Progress bar in the top during page loading.
# For more information: https://github.com/CodeByZach/pace
pace:
  enable: false
  # All available colors:
  # black | blue | green | orange | pink | purple | red | silver | white | yellow
  color: blue
  # All available themes:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: minimal

# Canvas ribbon
# For more information: https://github.com/hustcc/ribbon.js
canvas_ribbon:
  enable: false
  size: 300 # The width of the ribbon
  alpha: 0.6 # The transparency of the ribbon
  zIndex: -1 # The display level of the ribbon
```

## 四、NexT Tag Plugins

这个是 NexT 提供的一些很方便的小功能，详情见[官网介绍](https://theme-next.js.org/docs/tag-plugins)。

在这里我记录一些适用场景：

- [Link Grid](https://theme-next.js.org/docs/tag-plugins/link-grid.html)：框格链接，可以用来放友链，比如说[本站的友链界面](https://www.studentwei.xyz/links/)。
- [Mermaid](https://theme-next.js.org/docs/tag-plugins/mermaid.html)：新版本中[更新了该功能](https://github.com/next-theme/hexo-theme-next/issues/347)，可以直接用 Markdown 语法渲染 Memaid 图表，比如流程图、饼图等。
- [Tabs](https://theme-next.js.org/docs/tag-plugins/tabs.html)：分栏，可以用来做一些类别的介绍，节约空间，给读者选择的机会。我在”关于我“的书影音游的介绍中使用了分栏。
- [Note](https://theme-next.js.org/docs/tag-plugins/note.html)：标注，可以做一些特定的说明。

## 五、插件

### RSS feed

RSS 种子，方便博客关注者了解博客更新信息。

使用 `hexo-generator-feed` 插件，即可在 generate 时自动生成 RSS 种子。生成的位置为 `IP/atom.xml`。

```shell
npm install hexo-generator-feed
```

### Sitemap

站点地图，可以提交到搜索引擎的站长平台来增加收录。

使用 `hexo-generator-sitemap` 插件，即可在 generate 时自动生成站点地图。生成的位置为 `IP/sitemap.xml`。

```bash
npm install hexo-generator-sitemap
```

### Local search

本地搜索，为博客提供方便的搜索服务。

使用 `hexo-generator-searchdb` 插件，再修改 NexT 相关设置。

详情参考 [NexT 官方文档](https://theme-next.js.org/docs/third-party-services/search-services.html#Local-Search)。

```shell
npm install hexo-generator-searchdb
```

```yml
# Local Search
# Dependencies: https://github.com/next-theme/hexo-generator-searchdb
local_search:
  enable: true
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: false
```

### Pandoc

提供更好的 Markdown 渲染，主要有两点好处：脚注、MathJax。详情参考 [NexT 官方文档](https://theme-next.js.org/docs/third-party-services/math-equations.html)。

```bash
npm uninstall hexo-renderer-marked
npm install hexo-renderer-pandoc
```

### 插件统计

本博客所用到的所有插件汇总：

```shell
npm install hexo-deployer-git --save # 部署必需插件
npm install hexo-generator-feed
npm install hexo-generator-sitemap
npm install hexo-generator-searchdb
npm uninstall hexo-renderer-marked
npm install hexo-renderer-pandoc
```

## 六、更新

### 1. 更新博客内容

修改 `/source` 中的内容，调试内容可以在本地服务器查看：

```bash
hexo s
```

觉得没问题之后直接部署：

```bash
hexo clean
hexo g
hexo d
```

### 2. 更新 Hexo、NexT 本体

Hexo：直接使用 npm 更新组件。

```bash
npm update
```

NexT：直接在 NexT 文件夹下拉取更新 NexT。

记得先备份配置文件，然后参考 NexT 官方更新说明对配置文件进行修改。

```shell
cd themes/next
git pull
```

## 七、可能遇到的问题

- https://www.haoyizebo.com/posts/710984d0/

  ```shell
  hexo -g
  
  (node:2144) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
  (Use `node --trace-warnings ...` to show where the warning was created)
  (node:2144) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
  (node:2144) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
  (node:2144) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
  (node:2144) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
  (node:2144) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
  ```

- ```shell
  warning: LF will be replaced by CRLF in js/third-party/tags/pdf.js.
  ```

  https://www.jianshu.com/p/547e4a4d895e

  git 使用中 CRLF 将被 LF 替换问题

  ```shell
  git config --global core.autocrlf false
  ```

- TLS certificate verification has been disabled!

  https://stackoverflow.com/questions/66552941/github-tls-certificate-verification-has-been-disabled-on-windows



