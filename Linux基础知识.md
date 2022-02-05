# Linux基础知识

## CentOS7更换阿里yum源

```shell
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
yum clean all && yum makecache
```

## 清华大学镜像站

https://mirrors.tuna.tsinghua.edu.cn/

## 什么情况下会出现load高？多少算高？如何处理？



## 常见的配置文件详解

### /etc/selinux/config

## 查看服务器性能

### 