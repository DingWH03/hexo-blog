---
title: mariadb在archlinux上的安装与配置
author: DingWH03
hide: false
archive: false
math: false
date: 2025-01-11 23:36:14
tags: [数据库, mysql, mariadb]
categories:
- 简单记录
- 工具使用
- 数据库
---

## 一、安装

使用pacman方便地进行安装。

```bash
sudo pacman -S mariadb
```

## 二、配置

安装完之后，应跟随提示为mariadb配置basedir和datadir，一行命令即可：

```bash
sudo mariadb-install-db --user=username --basedir=/usr --datadir=/var/lib/mysql
```

> 别忘了将username换成自己的数据库用户名。

执行成功之后，终端也会给出提示，目前只有两个用户，一个是root@localhost，需要使用`sudo mysql`利用socket来登陆，另一个用户是自己创建的用户username@localhost，这个账户使用自己的系统账户`username`来登陆，然后来设置密码。这里建议直接使用`sudo mysql`登陆后，设置密码即可。

配置密码指令此处略过。

## 三、运行

```bash
sudo systemctl start mariadb
```

不过出现了错误：

```text
1月 11 23:33:01 dwhsarch mariadbd[6415]: 2025-01-11 23:33:01 0 [ERROR] InnoDB: The data file './ibdata1' must be writable
1月 11 23:33:01 dwhsarch mariadbd[6415]: 2025-01-11 23:33:01 0 [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.
1月 11 23:33:01 dwhsarch mariadbd[6415]: 2025-01-11 23:33:01 0 [Note] Plugin 'wsrep-provider' is disabled.
1月 11 23:33:01 dwhsarch mariadbd[6415]: 2025-01-11 23:33:01 0 [Note] Zerofilling moved table:  './mysql/plugin'
1月 11 23:33:01 dwhsarch mariadbd[6415]: 2025-01-11 23:33:01 0 [ERROR] Unknown/unsupported storage engine: InnoDB
1月 11 23:33:01 dwhsarch mariadbd[6415]: 2025-01-11 23:33:01 0 [ERROR] Aborting
1月 11 23:33:01 dwhsarch systemd[1]: mariadb.service: Main process exited, code=exited, status=1/FAILURE
1月 11 23:33:01 dwhsarch systemd[1]: mariadb.service: Failed with result 'exit-code'.
1月 11 23:33:01 dwhsarch systemd[1]: Failed to start MariaDB 11.6.2 database server.
```

为mysql数据目录授权即可

```bash
sudo chown -R mysql:mysql /var/lib/mysql
sudo chmod -R 750 /var/lib/mysql
```

再次启动运行的命令，顺利启动。

## 四、管理数据库

建议使用免费又好用的开源工具[DBeaver](https://github.com/dbeaver/dbeaver)。
