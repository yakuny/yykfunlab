+++
date = "2021-06-02"
author = "YYK"
title = "Termux - Android变身服务器 (二)"
categories = [
    "折腾",
]
tags = [
    "termux",
]
+++

上一篇《Termux - Android变身服务器 (一)》已经成功在 Android 手机上部署，并且简单验证了一些基础功能。这一篇就在 Termux 上搭建个网站吧，部署上 Wordpress 和 Typecho。

手机屏幕太小了，操作起来很不方便，所以整个安装过程是通过电脑 ssh 访问手机来完成的。先通过以下指令获取本地 ip，方便后续操作，例如我的是：**192.168.31.136**。如果是手机上操作，直接 127.0.0.1 就行了。
```basic
~ $ ifconfig wlan0
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.31.136  netmask 255.255.255.0  broadcast 192.168.31.255
...
```

在查找资料的过程中，发现了一篇博客，[Termux 高级终端安装使用配置教程](https://www.sqlsec.com/2018/05/termux.html#toc-heading-100)，堪称 Termux 百科全书，内容非常全。以下内容基本以我的需求，基于这篇博客精简（复制粘贴）出来的。

# 环境配置
> LNMP 安装及配置，L: Linux，N: Nginx, M: Mysql or MariaDB, P: PHP


需要留意一下，Termux 的路径跟普通 Linux 还是不太一样的。
```basic
~ $ echo $PREFIX
/data/data/com.termux/files/usr
~ $ ls $PREFIX
bin  etc  include  lib  libexec  opt  share  tmp  var
```

## 安装
```basic
# Nginx
pkg install nginx

# PHP
pkg install php

# PHP-FPM
pkg install php-fpm

# MariaDB
pkg install mariadb
```


## 配置 PHP-FPM
Nginx 本身不能处理 PHP，它只是个 Web 服务器，当接收到 PHP 请求后发给 PHP 解释器处理。Nginx 一般是把请求转发给 fastcgi 管理进程处理，PHP-FPM 是一个PHP FastCGI管理器。

编辑 php-fpm 的配置文件 www.conf：
```basic
vim $PREFIX/etc/php-fpm.d/www.conf
```
定位搜索 listen = 找到
```basic
listen = /data/data/com.termux/files/usr/var/run/php-fpm.sock
```
将其改成：
```basic
listen = 127.0.0.1:9000
```

## 配置 Nginx
```basic
vim $PREFIX/etc/nginx/nginx.conf
```

1. 添加 `index.php`到默认⾸⻚的规则⾥⾯

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22499123/1651504825462-b7b65e2b-36ec-4272-bbff-d5a94f155664.png#clientId=u76e61b38-3c81-4&from=paste&height=218&id=uf68585bf&originHeight=496&originWidth=1596&originalType=binary&ratio=1&rotation=0&showTitle=false&size=275760&status=done&style=none&taskId=ub1bc8282-9e8b-461d-b7a0-8403aef5faf&title=&width=700)

2. 取消 `location ~ \.php$` 这些注释，按照图⽚上的 提示修改：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22499123/1651504910992-9b4deaef-8369-4a57-bfec-37cdca9b6ac0.png#clientId=u76e61b38-3c81-4&from=paste&height=124&id=u059b2c82&originHeight=318&originWidth=1800&originalType=binary&ratio=1&rotation=0&showTitle=false&size=250126&status=done&style=none&taskId=u1daeb8c0-b57e-4072-9d88-56874a9c011&title=&width=700)

Nginx常用操作指令
```basic
# 启动, 默认端口 8080
~ $ nginx
# 重启
~ $ nginx -s reload
# 快速关闭
~ $ nginx -s stop
# 优雅关闭，完成所有请求后再退出
~ $ nginx -s quit
```

## 测试 PHP 解析
Termux ⾥⾯的 Nginx 默认⽹站的根⽬为：
```basic
/data/data/com.termux/files/usr/share/nginx/html
```
在这个⽹站根⽬录下新建 `info.php` 内容为: `<?php phpinfo(); ?>`
```basic
echo '<?php phpinfo(); ?>' > $PREFIX/share/nginx/html/info.php
```
依次启动 php-fpm服务 和 Nginx服务：
```basic
php-fpm
nginx
```
浏览器访问 `http://192.168.31.136:8080/info.php`(如果在手机上，IP替换为 127.0.0.1) 来看看刚刚新建的测试⽂件是否解析了：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22499123/1651546590215-5faef79e-ec7f-422c-87ec-9dff378143bc.png#clientId=u76e61b38-3c81-4&from=paste&height=436&id=u3410c915&originHeight=872&originWidth=1898&originalType=binary&ratio=1&rotation=0&showTitle=false&size=309955&status=done&style=none&taskId=u78c81eed-c58c-4e49-84c9-f9b60e6f107&title=&width=949)

## 配置 MariaDB
后台启动 Mysql 服务，使用`nohup`命令
 (nohup命令详细使用方式请看：[https://www.runoob.com/linux/linux-comm-nohup.html](https://www.runoob.com/linux/linux-comm-nohup.html))
```basic
nohup mysqld &
```
（如果需要停止 Mysql服务，命令如下）
```basic
kill -9 `pgrep mysql`
```
Termux 安装初始化数据库的时候包含两个⾼权限⽤户，⼀个是⽆法访问的 root ⽤户，另⼀个⽤户就是 Termux 的⽤户名，默认密码为空。通过 Termux用户 登录，修改 root 密码后，就可以正常登录 root 用户了，修改方式如下：
```basic
# 登录 Termux ⽤户
mysql -u $(whoami)
# 修改 root 密码的 SQL语句
use mysql;
set password for 'root'@'localhost' = password('你设置的密码');
# 刷新权限 并退出
flush privileges;
quit;
```
登录 root 用户
```basic
mysql -u root -p
Enter password: xxxxx（这⾥输⼊你的密码)

# 手动开启 root用户 的远程访问权限
grant all on *.* to root@'%' identified by '你的密码' with grant option;
flush privileges;
quit;
```

# 网站搭建
搭建网站前，别忘了先启动 `php-fpm`, `nginx`, `mysql`三个服务。
## 新建数据库
分别创建两个数据库：wordpress 和 typecho，供后面的网站安装使用。
```basic
~ $ mysql -u root -p
Enter password: xxxxx（这⾥输⼊你的密码)

> create database wordpress;
> create database typecho;
> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
| typecho            |
| wordpress          |
+--------------------+
7 rows in set (0.002 sec)
```

## WordPress

1. **下载安装包**
```basic
# wget 下载
wget https://cn.wordpress.org/latest-zh_CN.zip

# unzip 解压 替换自己的zip文件名
unzip wordpress-5.9.3-zh_CN.zip

# 将解压的⽂件夹移动到 nginx ⽹站根⽬录下
mv wordpress/ $PREFIX/share/nginx/html
```

2. **安装 WordPress**

浏览器访问 `http://192.168.31.136:8080/wordpress/`（使用手机本地操作的话，IP替换成 127.0.0.1）进行 WordPress 的安装。
```basic
数据库名: wordpress
用户名: root
密码: 自己设置的root密码
数据库主机：127.0.0.1 （不要使用localhost，会报错）
表前缀：不要去修改，默认就行
```

## Typecho

1. **下载安装包**
```basic
# wget 下载
wget https://github.com/typecho/typecho/releases/latest/download/typecho.zip

# unzip 解压 替换自己的zip文件名
unzip -d ./typecho typecho.zip

# 将解压的⽂件夹移动到 nginx ⽹站根⽬录下
mv typecho/ $PREFIX/share/nginx/html
```

2. **安装 Typecho**

浏览器访问 `http://192.168.31.136:8080/typecho/`（使用手机本地操作的话，IP替换成 127.0.0.1）进行 WordPress 的安装。
```basic
数据库适配器: Mysql原生函数适配器
数据库地址: 127.0.0.1 （不要使用localhost，会报错）
数据库用户名: root
数据库密码: 自己设置的root密码
数据库名: typecho
数据库前缀：不要去修改，默认就行
```

# 总结
到这里在手机上搭建网站就已经完成了，不过用手机当作服务器使用还存在两个问题：

1. 手机持续性充电运行是个非常危险的事情，潜在的安全隐患一定要重视；
2. 目前只能内网访问，如果想外网访问，必须做内网穿透。

第一个问题我找到两个解决方案：

1. 购买专业防火防爆箱，手机放箱子里，即使着火、爆炸也不怕；
2. 拆除旧电池，改成无电池直接供电。

防爆箱我是买不起的，只能使用方案二了。家里旧手机现在有四五个，找个最便宜的拆坏了也不心疼。等我改造完再写一篇。

关于内网穿透，网上有各种各样的方法，我还没想好用哪种方式更省钱合理。等想好了再说吧，现在也不急着用手机当服务器，一步步来，先改造手机。
