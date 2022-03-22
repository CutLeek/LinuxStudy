# Mysql安装

## 下载yum源

```shell
wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
```

## 安装Mysql的yum源

```shell
yum -y install mysql57-community-release-el7-10.noarch.rpm
```

## 修改配置文件

```
把/etc/yum.repos.d/mysql-community.repo里面的gpgcheck设置为0，不需要校验
```

## 安装Mysql

```shell
yum -y install mysql-community-server
```

## 启动Mysql

```
systemctl start mysqld
```

## 找到密码

```
cat /var/log/mysqld.log | grep password
```

## 登录数据库

```
拿着找到的密码
mysql -uroot -p
```

## 修改初始密码

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'your password';
```

# 如何在一台机器上启动两个mysql

1、卸载原有的mariadb

```shell
rpm -e --nodeps `rpm -qa |grep mariadb`
```

2、下载包

```shell
wget https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/mysql-5.7.35-linux-glibc2.12-x86_64.tar.gz --no-check-certificate
```

3、添加用户和用户组

```shell
groupadd mysql
useradd -g mysql mysql
```

4、创建目录

```shell
mkdir /soft
```

5、解压包

```shell
tar xvf mysql-5.7.35-linux-glibc2.12-x86_64.tar.gz -C /soft/
```

6、修改目录名称

```shell
mv /soft/mysql-5.7.35-linux-glibc2.12-x86_64/ /soft/mysql
```

7、创建目录

```shell
mkdir -p /soft/mysql/data/{3306,3307,log}
mkdir /soft/mysql/data/3306/{data,log,tmp}
mkdir /soft/mysql/data/3307/{data,log,tmp}
```

8、修改目录权限

```shell
chown -R mysql:mysql /soft/mysql
```

9、创建my.cnf配置文件

```shell
vim /etc/my.cnf
```

```shell
# 启动多个mysql实例
[client] 
default-character-set = utf8

[mysqld_multi] 
mysqld = /soft/mysql/bin/mysqld_safe
mysqladmin = /soft/mysql/bin/mysqladmin
log = /soft/mysql/data/log/mysqld_multi.log

[mysqld3306] 
mysqld=mysqld
mysqladmin=mysqladmin
datadir=/soft/mysql/data/3306/data
port=3306
user=mysql
performance_schema = off
server_id=3306
socket=/soft/mysql/data/3306/mysql_3306.sock
bind_address = 0.0.0.0                  #设置监听IP地址
skip-name-resolve = 0                 #关闭DNS反向解析 
log-output=file
slow_query_log = 1
long_query_time = 1
slow_query_log_file = /soft/mysql/data/3306/log/slow.log
log-error = /soft/mysql/data/3306/log/error.log
binlog_format = mixed
log-bin = /soft/mysql/data/3306/log/mysql3306_bin
explicit_defaults_for_timestamp=true
lower_case_table_names = 1
  
[mysqld3307] 
mysqld=mysqld
mysqladmin=mysqladmin
datadir=/soft/mysql/data/3307/data
port=3307
user=mysql
performance_schema = off
server_id=3307
socket=/soft/mysql/data/3307/mysql_3307.sock
bind_address = 0.0.0.0                  #设置监听IP地址
skip-name-resolve = 0                 #关闭DNS反向解析 
log-output=file
slow_query_log = 1
long_query_time = 1
slow_query_log_file = /soft/mysql/data/3307/log/slow.log
log-error = /soft/mysql/data/3307/log/error.log
binlog_format = mixed
log-bin = /soft/mysql/data/3307/log/mysql3307_bin
explicit_defaults_for_timestamp=true
lower_case_table_names = 1
```

10、添加环境变量

```shell
vim /etc/profile
```

```shell
在最后添加一行
PATH=$PATH:/soft/mysql/bin
```

11、初始化数据库

```shell
/soft/mysql/bin/mysqld --initialize --user=mysql --basedir=/soft/mysql/ --datadir=/soft/mysql/data/3306/data

/soft/mysql/bin/mysqld --initialize --user=mysql --basedir=/soft/mysql/ --datadir=/soft/mysql/data/3307/data
```

12、启动数据库

```shell
/soft/mysql/bin/mysqld_multi start
```

tips:启动单个实例

```shell
# /soft/mysql/bin/mysqld_multi start 3306
# /soft/mysql/bin/mysqld_multi start 3307
```

