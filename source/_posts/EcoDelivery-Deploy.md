---
title: EcoDelivery 系统部署在腾讯云
date: 2020-05-27 20:37:57
categories: 
tags: [python, flask, nginx]
---

EcoDelivery 智慧物流配送项目，采用 Flask 架构搭建。本文以腾讯云服务器为例，讲解部署流程。
<!--more-->

# 服务器实例
## 基本信息
* 区域：重庆一区
* CPU：1核
* RAM：1GB
* 公网带宽：1Mbps
* 操作系统：CentOS 7.2 64位

## 开放端口
调试过程中会用到`9090`端口和`9091`端口（仅作示例，可任意指定）。在腾讯云服务器上需要手动配置安全组开放端口。步骤如下：
1. 打开控制台，找到“实例”页面，找到相应的云服务器实例
2. 在表格中最右侧的“操作”列，找到“更多”-“安全组”-“配置安全组”
3. 点击启用的安全组的名称，进入安全组规则设置页面
4. 在“安全组规则”-“入站规则”选项卡中，点击“添加规则”
5. 添加两条规则，内容如下。
   
   | 类型   | 来源      | 协议端口 | 策略 |
   | ------ | --------- | -------- | ---- |
   | 自定义 | 0.0.0.0/0 | 9090     | 允许 |
   | 自定义 | 0.0.0.0/0 | 9091     | 允许 |

6. 添加后点击“完成”

# 安装 Python 3.7.5
## 安装依赖环境

```shell
    [root@VM ~]# yum install gcc-c++
    [root@VM ~]# yum -y install -y lsb
    [root@VM ~]# yum -y install -y libXScrnSaver
    [root@VM ~]# yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
```
## 下载 Python 3.7.5
从官网下载：
```shell
    [root@VM ~]# wget https://www.python.org/ftp/python/3.7.5/Python-3.7.5.tgz
```

*如果官网下载速度不理想，可以使用国内镜像源代替：*
```shell
    [root@VM ~]# wget https://mirrors.huaweicloud.com/python/3.7.5/Python-3.7.5.tgz
```

下载完成后解压源代码
```shell
    [root@VM ~]# tar -xvzf Python-3.7.5.tgz
    [root@VM ~]# cd Python-3.7.5/
```

## 编译安装

添加配置信息
```shell
    [root@VM Python-3.7.5]# ./configure --prefix=/usr/python3 --enable-optimizations --with-ssl
```

编译并安装（**需要的时间较长，视系统配置需要十分钟左右**）
```shell
    [root@VM Python-3.7.5]# make && make install
```

设置软链接
```shell
    [root@VM Python-3.7.5]# ln -s /usr/python3/bin/python3 /usr/bin/python3
    [root@VM Python-3.7.5]# ln -s /usr/python3/bin/pip3 /usr/bin/pip3
```

检查安装版本
```shell
    [root@VM Python-3.7.5]# python3 --version
```

升级pip3
```shell
    [root@VM Python-3.7.5]# pip3 install --upgrade pip
```
*如果官网下载速度不理想，可以使用国内镜像源代替：*
```shell
    [root@VM Python-3.7.5]# pip3 install -i --upgrade pip
```

(可选)将pip3安装源设置为国内镜像:
```shell
    [root@VM Python-3.7.5]# pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

# 安装项目文件

## 创建项目目录

回到根目录创建项目文件夹`/root/ecodelivery`
```shell
    [root@VM ~]# mkdir ecodelivery
    [root@VM ~]# cd ecodelivery
```

## 复制项目文件
将项目文件复制到 `/root/ecodelivery` 文件夹下

## 安装虚拟环境

安装virtualenv
```shell
    [root@VM ecodelivery]# pip3 install virtualenv
```

创建虚拟环境
```shell
    [root@VM ecodelivery]# /usr/python3/bin/virtualenv venv
```

激活虚拟环境
```shell
    [root@VM ecodelivery]# source venv/bin/activate
```

## 安装 Flask 及项目依赖项
```shell
    (venv) [root@VM ecodelivery]# pip install -r requirements.txt
```

# 安装并配置 uWSGI 
## 安装 uWSGI
```shell
    (venv) [root@VM ecodelivery]# pip install uwsgi
```
测试 uWSGI 是否安装成功：
```shell
    (venv) [root@VM ecodelivery]# uwsgi --http :9090 --wsgi-file uwsgi-test.py
```
访问服务器公网ip的9090端口，若显示“Hello World!”字样，说明端口可以正常访问，且 uWSGI 配置正确。

## 启动 uWSGI
uWSGI 相关配置在项目文件夹内的 uwsgi.ini 文件内，项目打包发布时已经做了基本配置，可根据需要自行修改
```shell
    (venv) [root@VM ecodelivery]# uwsgi --ini uwsgi.ini
```

如果希望退出SSH会话后系统仍然提供服务，请使用`nohup`命令启动
```shell
    (venv) [root@VM ecodelivery]# nohup uwsgi --ini uwsgi.ini
```
## 设置 uWSGI 开机启动
赋予uwsgi服务管理脚本可执行权限
```shell
    [root@VM ~]# chmod -x /root/ecodelivery/uwsgid.sh
```
将服务配置文件复制到`systemd`配置文件夹下，赋予754权限
```shell
    [root@VM ~]# cd /usr/lib/systemd/system
    [root@VM system]# cp /root/ecodelivery/uwsgid.service uwsgid.service
    [root@VM system]# chmod 754 uwsgid.service
```
开启服务
```shell
    [root@VM ~]# systemctl enable uwsgid.service
```

# 安装并配置 Nginx
## 安装 Nginx
```shell
    [root@VM ~]# yum install nginx
```

检查 Nginx 安装版本
```shell
    [root@VM ~]# nginx -V
```

## 配置 Nginx
Nginx 相关配置在项目文件夹内的 `ecodelivery.conf` 文件内，运行前请打开并修改其中的 `server_name` 项为自己的服务器公网ip

运行 Nginx 进行测试
```shell
    [root@VM ~]# nginx -c /root/ecodelivery/ecodelivery.conf
```

打开浏览器，访问服务器的 `9090` 端口，如果能正常显示系统登录页面，说明安装配置正确。否则请检查之前步骤中的错误。

## 设置 Nginx 服务启动参数
打开`/lib/systemd/system/nginx.service`文件，找到其中的`ExecStart`项，在原来的命令后面添加
` -c /root/ecodlivery/ecodelivery.conf`参数，以使用项目指定的配置文件启动 Nginx

# 系统初始化
## 初始化数据库
打开项目文件夹，进入虚拟环境，执行命令
```shell
    [root@VM ~]# cd /root/ecodelivery
    [root@VM ecodelivery]# source venv/bin/activate
    [root@VM ecodelivery]# flask initdb
```
生成的默认管理员工号为`0000`，密码为`123456`，请进入系统后尽快修改密码

## 初始化地图
对数据库进行初始化后，由于所有路点的默认初始坐标为`(0,0)`，在地图上无法正常显示，需要对地图上的点进行位置初始化：
1. 以管理员身份登录系统，打开“地图管理”
2. 在“地图概览”一栏中点击“编辑”按钮，进入地图编辑模式
3. 点击“手动”按钮，切换到自动布局模式，此时路网会依据力引导布局算法自动布局，待稳定后保存路网即可。