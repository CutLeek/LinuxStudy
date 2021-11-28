Git命令

简单了解一下git

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

```
 Workspace：工作区
 Index / Stage：暂存区
 Repository：仓库区（或本地仓库）
 Remote：远程仓库
```



1、初始化一个本地代码库

```
git init 
```

2、从远端克隆代码到本地

```
git clone https://github.com/CutLeek/LinuxStudy.git
```

3、将本地写好的代码添加到暂存区

```
添加一个或多个文件到暂存区：
git add [file1] [file2] ...

添加指定目录到暂存区
git add [dir]

添加当前目录下的所有文件到暂存区
git add .
```

4、配置个人名称和电子邮件信息以及忽略windows与linux、max换行符不同的问题

```
git config --global user.name "CutLeek"
git config --global user.email "kirobot@163.com"
git config --global core.autocrlf false
```

5、将暂存区的代码提交到本地仓库

```
git commit -m "本次修改的描述信息"
```

6、将本地仓库的代码推到远程仓库

```
git push origin master
```

























