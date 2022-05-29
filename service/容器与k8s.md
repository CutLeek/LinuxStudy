# 容器与k8s

docker的安装

1、安装依赖

```shell
yum -y install gcc gcc-c++ yum-utils device-mapper-persistent-data lvm2
```

2、设置docker源

```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

3、查看yum源中的docker的版本

```shell
yum list docker-ce.x86_64 --showduplicates | sort -r 
```

可以指定版本安装，也可以直接安装最新版

4、安装docker

```
yum -y install docker-ce
```

5、启动docker

```shell
systemctl start docker
```

