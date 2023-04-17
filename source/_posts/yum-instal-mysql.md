---
title: 红帽系发行版官方安装 MySQL
toc: true
date: 2023-04-17 08:40:37
categories:
- 运维笔记
tags:
- Linux
- RedHat
- 运维
- MySQL
---

NOTE：这是一篇运维方面的笔记，原文存档在 Linux-notes GitHub Repo 中，适用于 MySQL 8.0。

EL 8/9 版本可以用 dnf 命令代替 yum。

<!--more-->

请参阅：[2.5.1 使用 MySQL Yum 存储库在 Linux 上安装 MySQL](https://dev.mysql.com/doc/refman/8.0/en/linux-installation-yum-repo.html)

## 准备工作

可以考虑通过 `sudo yum makecache && sudo yum upgrade ` 的方式先更新一下系统，但是已经投入生产的环境不能轻易升级系统。



## 添加存储库

主服务器配置：打开 MySQL 的配置文件，设置服务器ID以及启用二进制日志。

进入官网下载 rpm 包：https://dev.mysql.com/downloads/repo/yum/ ，然后进行安装：

```shell
sudo yum install platform-and-version-specific-package-name.rpm

#刷新软件源
sudo yum makecache
```

检查是否已经顺利启用：

```
sudo yum repolist enabled | grep "mysql.*-community.*"
```

> 警告：一旦在你的系统上启用了 MySQL Yum 仓库，任何通过 yum update 命令进行的全系统更新（或者对于启用 dnf 的系统来说，dnf upgrade）都会升级你系统上的 MySQL 包，并替换任何原生的第三方包。



## 配置存储库

根据需要启用或禁用 MySQL 5.7 或 8.0 的源。

```shell
# 在新系统上，用 dnf config-manager 代替 yum-config-manager
sudo yum-config-manager --disable mysql57-community
sudo yum-config-manager --enable mysql80-community
```

#### 禁用默认的 MySQL 模块

（仅限 EL8 系统）基于 EL8 的系统，如 RHEL8 和 Oracle Linux 8 包含一个默认启用的 MySQL 模块。 除非禁用此模块，否则它会屏蔽 MySQL 存储库。禁用包含的模块并使 MySQL 存储库包可见，请使用以下内容 命令（对于启用了 DNF 的系统，将命令中的 **yum** 替换为 **DNF**）：

```
sudo yum module disable mysql
```



## 正式安装

运行命令：

```shell
sudo yum install mysql-community-server
```

