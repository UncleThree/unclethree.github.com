---
layout: post
title: 【Mac日常系列】Homebrew方式安装的Mysql忘记密码后的操作
date: 2017-03-29 21:30:22
category: "Mac日常系列"
---
![banner](https://cdn.pixabay.com/photo/2017/03/29/08/19/usa-2184468__480.jpg "mac")

> Mac日常系列，通过 Hombrew 方式安装的 Mysql，在使用一段时间过后，忘记 Mysql 密码问题时操作指南

使用 Homebrew 安装 Mysql 

> brew install mysql

查看通过 Homebrew 方式安装的相关信息,

> brew info mysql 

了解所在目录

>/usr/local/Cellar/mysql/5.7.17

第一次安装，可执行 Mysql 提供的配置脚本 *mysql_secure_installation*

>/usr/local/opt/mysql/bin/mysql_secure_installation //mysql 提供的配置向导

依次执行如下动作：

1. Set root password
2. Remove anonymous users
3. Disallow root login remotely
4. Remove test database and access to it
5. Reload privilege tables now

以上为安装初始化过程。

由于已执行过初始配置，且忘记 Root 密码。该场景下

1. 确认配置文件信息 [^one]

> mysqld --help --verbose \| more (查看帮助)

确认配置文件默认读取顺序：

> Default options are read from the following files in the given order: /etc/my.cnf /etc/mysql/my.cnf /usr/local/etc/my.cnf ~/.my.cnf

通常所给出位置没有配置文件，手动新建

>touch ~/.my.cnf
 

2．修改 MySQL 登录设置
在 [mysqld] 的段中加上一句：**skip-grant-tables**

```
# vi ~/.my.cnf 
例如： 
[mysqld] 
datadir=/var/lib/mysql 
socket=/var/lib/mysql/mysql.sock 
skip-grant-tables 
```

3．重启服务[^two]

> brew services restart mysql 

4．登录并修改 MySQL root 用户密码 


```
mysql

mysql> USE mysql; 

mysql> update mysql.user set authentication_string=password('new_pass') where user='root' and Host = 'localhost'; 

Query OK, 0 rows affected (0.00 sec) 
Rows matched: 2 Changed: 0 Warnings: 0 

mysql> flush privileges ; 

Query OK, 0 rows affected (0.01 sec) 

mysql> quit

```

5．恢复 MySQL 登录设置

```
# vi ~/.my.cnf 
将刚才在 [mysqld] 的段中加上的 skip-grant-tables 删除 
```

6．重启服务

> brew services restart mysql



[^one]:初始化安装 Mysql 时另一种配置设置方式：复制推荐配置ls $(brew --prefix mysql)/support-files/my-* (用这个可以找到样例.cnf)cp /usr/local/opt/mysql/support-files/my-default.cnf /etc/my.cnf (拷贝到第一个默认读取目录)

[^two]: brew services start/stop mysql (启动)

------

原创文章转载请注明出处：[Mac下以Homebrew所安装Mysql忘记密码后的操作](https://unclethree.github.io/mac日常系列/2017/03/29/mac-map-for-lose-mysql-pwd.html)
