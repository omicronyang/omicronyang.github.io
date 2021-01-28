---
title: 设置-Sublime Text 3 开发环境
date: 2020-01-23 22:15:42
categories: 开发设置
tags: [python, sublimetext]
---
轻量化、插件多、功能强大，sublime text 3 担任首席文本编辑工具一点不为过
<!-- more -->
## 下载安装
官网地址：[https://www.sublimetext.com/3](https://www.sublimetext.com/3)

### 安装包管理器：`package control` 

现在只需要在命令面板`ctrl+shift+p`运行`Install Package Control`即可

### 右键菜单
如果在安装时没有勾选“添加到右键菜单”，可以手动为其添加右键打开文件或文件夹的功能

新建一个`.reg`文件，在其中输入以下内容：
```re
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\Open with Sublime Text]
@="用 Sublime Text 打开"
"icon"="\"C:\\Program Files\\Sublime Text 3\\sublime_text.exe\""

[HKEY_CLASSES_ROOT\*\shell\Open with Sublime Text\command]
@="\"C:\\Program Files\\Sublime Text 3\\sublime_text.exe\" \"%1\""

[HKEY_CLASSES_ROOT\Directory\shell\Open with Sublime Text]
@="用 Sublime Text 打开"
"icon"="\"C:\\Program Files\\Sublime Text 3\\sublime_text.exe\""

[HKEY_CLASSES_ROOT\Directory\shell\Open with Sublime Text\command]
@="\"C:\\Program Files\\Sublime Text 3\\sublime_text.exe\" \"%V\""

[HKEY_CLASSES_ROOT\Directory\Background\shell\Open with Sublime Text]
@="用 Sublime Text 打开"
"icon"="\"C:\\Program Files\\Sublime Text 3\\sublime_text.exe\""

[HKEY_CLASSES_ROOT\Directory\Background\shell\Open with Sublime Text\command]
@="\"C:\\Program Files\\Sublime Text 3\\sublime_text.exe\" \"%V\""
```
如果不是安装在默认路径，内容要修改为对应的安装路径。保存时记得以 `UTF-16 LE` 编码保存，否则中文会乱码


## 主题
### [Material Theme](https://packagecontrol.io/packages/Material%20Theme)

下载：`Package Control: Install Package` -> `Material Theme`

## 插件

### [Color Highlight](https://packagecontrol.io/packages/Color%20Highlight)

自动显示css中颜色代码的颜色

下载：`Package Control: Install Package` -> `Color Highlight`

### [Emmet](https://packagecontrol.io/packages/Emmet)

代码工具

下载：`Package Control: Install Package` -> `Emmet`

### [SideBarEnhancements](https://packagecontrol.io/packages/SideBarEnhancements)

左侧目录增强工具

下载：`Package Control: Install Package` -> `SideBarEnhancements`

### [Jinja2](https://packagecontrol.io/packages/Jinja2)

下载：`Package Control: Install Package` -> `Jinja2`

项目开发的时候用到，提供Jinja2模板语法支持

## 偏好设置

### 主设置
``` json
{
	"color_scheme": "Packages/Material Theme/schemes/Material-Theme.tmTheme",
	"font_size": 14,
	"highlight_line": true,
	"ignored_packages":
	[
		"Markdown",
		"Vintage"
	],
	"line_padding_bottom": 2,
	"line_padding_top": 2,
	"material_theme_accent_scrollbars": true,
	"material_theme_accent_sky": true,
	"material_theme_big_fileicons": true,
	"material_theme_bullet_tree_indicator": true,
	"material_theme_compact_sidebar": true,
	"material_theme_contrast_mode": true,
	"material_theme_small_statusbar": true,
	"material_theme_small_tab": true,
	"material_theme_tabs_autowidth": true,
	"save_on_focus_lost": true,
	"theme": "Material-Theme.sublime-theme",
	"word_wrap": false
}
```
