---
title: 设置-个人开发环境搭建
date: 2020-01-14 23:40:01
categories: 开发设置
tags: [python, nodejs, visual studio]
---

自己做的.NET程序比较多，一般使用 Visual Studio IDE 环境，再加上2017开始有免费的 VS Community 版本，对于小型项目来说完全够用。而且它采用组件化的管理安装，集成了python、nodejs等组件的安装，安装过程比较轻松简单，就是安装之后还要配置一下环境变量。至于常用的文本编辑器则选择功能强大的 VS Code。鉴于最近几次重装系统的经验，基于最新的VS2019版本，记录一下新系统搭建个人常用开发环境的过程。
<!--more-->

# 安装 Visual Studio, Python, node.js 和 Git
## 通过 Visual Studio Installer 安装组件
VS Community版本是免费的，可以直接在[官网](https://visualstudio.microsoft.com/)下载。

下载下来之后是一个安装器（**Visual Studio Installer**，以后组件维护就靠这个），勾选自己需要的组件安装即可。我个人的选择：
- 工作负载
	- Python 开发
	- Node.js 开发
	- .NET 桌面开发
	- 通用 Windows 平台开发
- 单个组件
	- 适用于 Visual Studio 的 GitHub 扩展
	- 适用于 Windows 的 Git

其中 **Python 开发** 勾选之后会自动选择适合系统版本的 **Python3**, 如果需要其它版本的 **Python3** 或者 **Python2**，可以在右边列出的包里面勾选。

选择好之后下面的**位置**里面可以设置安装路径，然后右下角选择**下载时安装**（在网络比较好的情况下可以边下边装，节省一点时间），然后点击**安装**就可以了。

安装过程中，有可能下载进度会卡在**98%**左右，估计是某些组件（安卓相关）的下载需要科学上网，可以点**暂停**，科学后**继续**就可以了。

## 配置环境变量
打开设置，搜索“环境变量”，编辑系统环境变量。

在系统环境变量的`PATH`（大小写不敏感）中添加以下几条路径：
```
C:\Program Files\Git\cmd
C:\Program Files\Git\bin
C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python37_64
C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python37_64\Scripts
C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Microsoft\VisualStudio\NodeJs
```

这里因为我安装的是Python3.7 64位版本，文件夹名字就是Python37_64，其它版本的略有不同。其中`C:\Program Files (x86)\Microsoft Visual Studio`是VS的默认安装路径，`\2019\Community\`是 VS 2019 Community 版本的安装路径，如果安装的是其它版本的VS（2017及以后），路径形式应该相类似，视具体情况自行改动。

为了在命令行里面用npm，还要再做一步，把上述第五条路径下`\node_modules\npm\bin`里面的`npm`文件复制到`NodeJs`文件夹下，这样才可以使用npm命令安装。

## 配置 Git 用户信息
打开 `git bash` ，运行以下两条命令
```bash
$ git config --global user.name "用户名"
$ git config --global user.email "用户邮箱"
```

# 安装 Visual Studio Code
## 通过官网安装 Visual Studio Code
Visual Studio Code 是微软推出的功能强大的文本编辑器，在各路插件的加持下已经不输很多，使用体验大大提升，尤其是集成多种终端尤为方便。[官网地址](https://code.visualstudio.com/Download)

安装程序运行完成后，记得勾选两个 **“添加到右键菜单”** 的选项，方便项目调试和使用终端

## 配置 Visual Studio Code

### 中文UI
`Ctrl+Shift+P -> Configure Display Language`，选择`Install additional language...`，会在左侧扩展一栏中显示所有语言包，找到简体中文，下载安装后重启VSCode即可

### 环境设置
`Ctrl+Shift+P -> Preferences: Open Settings (JSON)` 打开json格式设置文件，添加以下内容：
```json
{
	//颜色主题
	"workbench.colorTheme": "Monokai",
	
	//保存时自动格式化
	"editor.formatOnSave": true,
	
	//窗口失去焦点时自动保存
	"files.autoSave": "onWindowChange",
	
	//代码参数提示
	"editor.parameterHints.enabled": true,
	"editor.quickSuggestions": {
		"other": true,
		"comments": true,
		"strings": true
	},
	
	//关闭 Alt 快捷键打开主菜单
	"window.enableMenuBarMnemonics": false,
	
	//启用导航路径
	"breadcrumbs.enabled": true,
	
	//关闭左侧导航栏删除确认
	"explorer.confirmDelete": false,
}
```

### 插件安装
1. [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) - Python 语言支持
2. [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) - Markdown 增强
3. [Matlab](https://marketplace.visualstudio.com/items?itemName=Gimly81.matlab) - Matlab 语言支持
4. [hexdump for VSCode](https://marketplace.visualstudio.com/items?itemName=slevesque.vscode-hexdump) - 16进制文件查看器
5. [open in browser](https://marketplace.visualstudio.com/items?itemName=techer.open-in-browser) - 在浏览器中打开（预览）
6. [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css) - 提供 css 样式的代码提示
7. [Color Highlight](https://marketplace.visualstudio.com/items?itemName=naumovs.color-highlight) - 代码内颜色速览
8. [Material Theme Icons](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-material-theme-icons) - 美化图标

安装完成后在设置中添加如下项
```json
{
	//(Python)Python解释器路径
	"python.pythonPath": "C:\\Program Files (x86)\\Microsoft Visual Studio\\Shared\\Python37_64\\python.exe",

	//(Python)隐藏Python插件的启动页
	"python.showStartPage": false,
	
	//(Color Highlight)颜色单词匹配
	"color-highlight.matchWords": true,
	
	//(Color Highlight)颜色显示为下划线
	"color-highlight.markerType": "underline",
	
	//(Material Theme Icons)设置图标
	"workbench.iconTheme": "eq-material-theme-icons",
}
```

# 安装 Windows Terminal
## 通过微软商店安装 Windows Terminal
Windows Terminal 是微软出品的一个强大的命令行集成工具，同时给各个命令行添加了颜色主题、快捷键等等的支持，非常好用，而且安装非常方便，打开微软商店页面安装即可。[商店页面](https://aka.ms/terminal) | [项目地址](https://github.com/microsoft/terminal)

## 配置 Windows Terminal
安装完成后打开 Windows Terminal，在顶部下拉箭头菜单中点击**设置**，或`Ctrl+,`打开配置文件`settings.json`，参照以下内容进行修改

```json
"profiles":
{
	"defaults":
	{
		// Put settings here that you want to apply to all profiles.
		// 添加在此处运行的支持
		"startingDirectory": ".",
		// 设置颜色主题和字体
		"colorScheme": "One Half Dark",
		"fontFace" : "Consolas"
	},
	"list":
	[
		{
			// Make changes here to the cmd.exe profile.
			"guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
			"name": "Command Prompt",
			"commandline": "cmd.exe",
			"hidden": false
		},
		{
			// Make changes here to the powershell.exe profile.
			"guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
			"name": "Windows PowerShell",
			"commandline": "powershell.exe",
			"hidden": false
		},
		{
			// Git Bash 配置
			"guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6109}",
			"name": "Gitbash",
			"commandline": "bash.exe",
			"icon": "C:\\Program Files\\Git\\mingw64\\share\\git\\git-for-windows.ico",
			"hidden": false
		},
		{
			"guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
			"hidden": false,
			"name": "Azure Cloud Shell",
			"source": "Windows.Terminal.Azure"
		}
	]
},
```

## 设置右键菜单
下载[图标文件](https://raw.githubusercontent.com/microsoft/terminal/main/res/terminal.ico)，命名为 `wt.ico` 并放到以下路径：`%USERPROFILE%\AppData\Local\Microsoft\WindowsApps\`

新建一个`.reg`文件，输入以下内容并保存
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\wt]
@="在此处打开 Windows Terminal(&T)"
;设置图标
"Icon"="%USERPROFILE%\\AppData\\Local\\Microsoft\\WindowsApps\\wt.ico"
;设置仅在 Shift+右键 菜单中显示
;"Extended"="" 

[HKEY_CLASSES_ROOT\Directory\Background\shell\wt\command]
@="wt.exe"
```

## 命令行提权
使用 Windows Terminal 运行 Powershell，运行以下命令解除脚本运行限制及安装 **gsudo**
```ps
Set-ExecutionPolicy RemoteSigned -scope Process; iwr -useb https://raw.githubusercontent.com/gerardog/gsudo/master/installgsudo.ps1 | iex
```
安装完成后在原本执行的命令前加 **gsudo** 即可以管理员模式运行命令，或者执行 `gsudo cmd` / `gsudo powershell` / `gusudo bash` 以管理员模式运行命令行


# 安装 PowerToys
PowerToys 也是微软官方提供的 Win10 小工具合集，提供屏幕分区、颜色提取、超级命名、快速运行等功能，配合 Windows Terminal 可以获得非常流畅的体验。[项目地址](https://github.com/microsoft/PowerToys)
