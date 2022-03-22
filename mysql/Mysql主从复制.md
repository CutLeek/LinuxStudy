# Mysql主从复制

前提条件：

主库需要开始binlog日志，并且设置server-id，从库需要设置server-id，且跟主库要不相同

```shell
[mysqld]
log-bin=mysql-bin #开启二进制日志
server-id=1 #设置server-id
```

## 配置主库

### 添加同步用户

```mysql
mysql> CREATE USER 'rep'@'%' IDENTIFIED BY 'Mysql3306';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'rep'@'%';
mysql>flush privileges;
```

### 查看master状态

```mysql
 SHOW MASTER STATUS;
```

## 配置从库

### 设置主从关系

```mysql
mysql> CHANGE MASTER TO
    ->     MASTER_HOST='localhost',
    ->     MASTER_USER='rep',
    ->     MASTER_PASSWORD='Mysql3306',
    ->     MASTER_LOG_FILE='mysql-bin.000003',
    ->     MASTER_LOG_POS=73;
```

### 启动slave同步进程

```mysql
mysql>start slave;
```

### 查看同步状态

```mysql
show slave status\G;
```

```mysql
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
#这两个值都是yes，就说明是同步成功的，如果不是，请看error.log
```

## 一些命令

### 停止同步

```mysql
mysql>stop slave;
```

### 改变主从配置

```mysql
change master to master_log_file='mysql-bin.000288',master_log_pos=627625631
```











































