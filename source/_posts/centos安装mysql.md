---
title: CentOS安装mysql
date: 2018-04-17 09:21:38
tags:
---

# 安装

从yum里直接安装

```shell
yum install mysql
yum install mysql-devel
```

yum中没有`mysql-server`,所以从官网下载并安装

```shell
wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-community-server
```

重启mysql服务

```shell
service mysqld restart
```

# 配置

在`/etc/my.cnf`中配置字符集

```ini
[mysql]
default-character-set =utf8
```

设置`root`密码

```sql
set password for 'root'@'localhost' =password('password');
```

设置远程登陆

```sql
grant all privileges on *.* to root@'%'identified by 'password';
```

至此，mysql安装就全部完毕，可以使远程连接工具用`root`用户连接3306端口了。