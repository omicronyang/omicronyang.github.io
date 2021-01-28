---
title: 设置-Hexo+Next主题个人博客搭建
date: 2020-01-15 10:48:18
categories: 博客
tags: [hexo]
---

如你所见，本博客使用 hexo+github pages 搭建，本文记录一下搭建的过程hexo的基本用法
<!--more-->

# github pages 创建

# Hexo 安装与配置
## 安装 Hexo
需要首先安装 `nodejs`，可以参考之前的文章
安装完成后，在博客文件夹的上级目录打开命令行，运行以下命令
```bash
$ npm install hexo-cli -g
$ hexo init Blog
$ cd Blog
$ npm install
```
Hexo博客的基础架构已经在`Blog`文件夹内生成，该文件夹为**博客的根目录**

## Hexo 常用指令
- `hexo n(ew) [layout] "postName"`：后面跟随文章名，新建一篇文章，`[layout]`默认为`post`，存放在`source\_posts`下
- `hexo g(enerate)`：生成静态页面
- `hexo d(eploy)`：部署页面（上传）
	- `hexo d -g`：生成并部署
- `hexo s(erver)`：本地预览静态页面，默认为[http://localhost:4000](http://localhost:4000)
	- `hexo s -g`：生成并预览
- `hexo clean`：清除本地生成的静态页面

## Hexo 配置
在博客根目录打开`_config.yml`进行配置：
```yml
# _config.yml

# 站点设置
title: OMI=v=LOG
author: Omicron Yang
language: zh-CN
timezone: 'Asia/Shanghai'
url: http://omicronyang.github.io

# 代码块设置
highlight:
  enable: false
prismjs:
  enable: true
  preprocess: true
  line_number: true
  tab_replace: ''
```

# Next 主题安装与配置
[英文文档](https://theme-next.js.org/)
## Next 8.2.0 主题安装
在博客根目录打开命令行，运行以下命令
```bash
# 初次安装
$ npm install hexo-theme-next

# 安装指定版本
$ npm install hexo-theme-next@8.2.0

# 升级到最新版本
$ npm install hexo-theme-next@latest
```

## Next 主题设置
首先，打开根目录下的`_config.yml`，修改以下项以将主题设置为**Next**
```yml
# _config.yml

theme: next
```
然后在博客根目录执行以下命令：
```bash
# 复制设置文件模板
$ cp node_modules/hexo-theme-next/_config.yml _config.next.yml
```
然后打开`_config.next.yml`进行设置：
```yml
# _config.next.yml

# 暗黑模式（跟随系统）
darkmode: true

#左栏导航设置
menu_settings:
  icons: true
  badges: false

# 头像图片
avatar:
  url: #/images/avatar.gif
  rounded: true
  rotated: false

# 社交链接
social: #略
# 社交链接样式
social_icons:
  enable: true
  icons_only: true
  transition: false

# 文章导航
toc:
  enable: true
  # 自动加数字标号
  number: true
  wrap: false
  # 不自动折叠
  expand_all: true
  max_depth: 6

# 页脚设置
footer:
  # 起始年份，缺省值为当前年份
  since: 2019
  # 年份和版权信息之间的图标
  icon:
    name: fa fa-train
    animated: false
    color: "#000000"
  copyright:
  powered: true

# -------------文章设置-------------
# 标签前使用图标而不是井号
tag_icon: true
# 在线编辑文章
post_edit:
  enable: true
  # 以下链接指向查看代码页面
  # url: https://github.com/<用户名>/<仓库名>/tree/<分支名>/source/
  # 以下链接指向编辑代码页面
  # url: https://github.com/<用户名>/<仓库名>/edit/<分支名>/source/

# 代码块设置
codeblock:
  theme:
    light: monokai
    dark: monokai
  prism:
    light: prism-coy
    dark: prism-okaidia
  # Add copy button on codeblock
  copy_button:
    enable: true
    # Available values: default | flat | mac
    style: flat

# 回到开头
back2top:
  enable: true
  # 放在左栏导航下面
  sidebar: true
  # 显示百分比进度
  scrollpercent: true

# 图片放大查看
fancybox: true
# 懒加载
lazyload: true
```




# Hexo 插件安装
## Hexo Pangu
```bash
$ npm install hexo-pangu
$ hexo clean
```
安装后生成时可能报错`Could not parse CSS stylesheet`