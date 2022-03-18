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

