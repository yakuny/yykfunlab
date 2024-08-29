+++
date = "2021-06-01"
author = "YYK"
title = "Termux - Android变身服务器 (一)"
categories = [
    "折腾",
]
tags = [
    "termux",
]
+++

# Termux
官网地址：[https://termux.com/](https://termux.com/)

Termux 是一个 Android 终端模拟器，Linux 环境 APP，开源且不需要 root。Termux 自带一个小的基础系统，安装 APP 即可使用。

# 安装
官网提供了两个安装方式：

1. GitHub：[https://github.com/termux/termux-app#github](https://github.com/termux/termux-app#github)
2. F-Droid：[https://f-droid.org/en/packages/com.termux/](https://f-droid.org/en/packages/com.termux/)

打开软件后会跳出如下界面：
```basic
Welcome to Termux!

Community forum: https://termux.com/community
Gitter chat:     https://gitter.im/termux/termux
IRC channel:     #termux on libera.chat

Working with packages:

  * Search packages:   pkg search <query>
  * Install a package: pkg install <package>
  * Upgrade packages:  pkg upgrade

  Subscribing to additional repositories:

  * Root:     pkg install root-repo
  * X11:      pkg install x11-repo

  Report issues at https://termux.com/issues

~ $
```

查看下预装的操作系统
```python
~ $ uname -a
Linux localhost 4.4.23+ #1 SMP PREEMPT Wed Nov 7 15:09:22 CST 2018 aarch64 Android
```

# 切换Termux源
为了流畅使用，还是需要切换源的。在较新版的 Termux 中，官方提供了图形界面（TUI）来半自动替换镜像，推荐使用该种方式以规避其他风险。在 Termux 中执行如下命令：
```python
~ $ termux-change-repo
```
在图形界面引导下，使用自带方向键可上下移动。
第一步使用空格选择需要更换的仓库，之后在第二步选择 TUNA/BFSU 镜像源。确认无误后回车，镜像源会自动完成更换。

更多 Termux 镜像使用帮助：[https://mirrors.tuna.tsinghua.edu.cn/help/termux/](https://mirrors.tuna.tsinghua.edu.cn/help/termux/)

换好源后顺手再升级一下
```python
~ $ apt update && apt upgrade
```

# 通过SSH连接Android设备

1. **设置密码**
```python
~ $ passwd
```

2. **安装软件包**
```python
~ $ pkg install openssh
```

2. **启动 OpenSSH server**
```python
~ $ sshd
```
停止方式：
```python
~ $ pkill sshd
```

3. **查询用户名、IP 及 端口**
```python
~ $ whoami
u0_a131

~ $ ifconfig wlan0
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.31.97  netmask 255.255.255.0  broadcast 192.168.31.255

~ $ netstat -ntlp | grep sshd
tcp        0      0 0.0.0.0:8022            0.0.0.0:*               LISTEN      22567/sshd
tcp6       0      0 :::8022                 :::*                    LISTEN      22567/sshd
```
通过查询获取的信息为：

- 用户名：u0_a131
- IP：192.168.31.97
- 端口：8022

4. **ssh远程访问**
```python
$ ssh -p <PORT> <USER>@<IP>

$ ssh -p 8022 u0_a131@192.168.31.97
```

# **安装 Python 写两行**
```python
~ $ pkg search python
python/stable,now 3.10.4 aarch64 [installed]
  Python 3 programming language intended to enable clear programs
...

~ $ pkg install python
...

~ $ python3
...
>>> print("hello world")
hello world
>>> exit()

~ $ pip3 -V
pip 22.0.4 from /data/data/com.termux/files/usr/lib/python3.10/site-packages/pip (python 3.10)

```
