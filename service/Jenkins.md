# Jenkins

### docker部署jenkins

1、拉镜像

```shell
docker pull jenkins/jenkins:lts
```

2、创建volume目录

```shell
mkdir -p /opt/docker/jenkins
```

3、修改目录权限

```shell
chown -R jenkins:jenkins /opt/docker/jenkins
```

4、启动容器

```shell
docker run -di --name=jenkins -p 8080:8080 -v /opt/docker/jenkins:/var/jenkins_home jenkins/jenkins:lts
```

### 插件配置



### maven项目的配置



### docker类型的项目配置

