# Git命令

### 简单了解一下git

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

```
 Workspace：工作区
 Index / Stage：暂存区
 Repository：仓库区（或本地仓库）
 Remote：远程仓库
```



#### 1、初始化一个本地代码库

```
git init 
```

#### 2、从远端克隆代码到本地

```
git clone https://github.com/CutLeek/LinuxStudy.git
```

#### 3、将本地写好的代码添加到暂存区

```
添加一个或多个文件到暂存区：
git add [file1] [file2] ...

添加指定目录到暂存区
git add [dir]

添加当前目录下的所有文件到暂存区
git add .
```

#### 4、配置个人名称和电子邮件信息以及忽略windows与linux、max换行符不同的问题

```
git config --global user.name "CutLeek"
git config --global user.email "kirobot@163.com"
git config --global core.autocrlf false
```

#### 5、将暂存区的代码提交到本地仓库

```
git commit -m "本次修改的描述信息"
```

#### 6、将本地仓库的代码推到远程仓库

```
git push origin master
```

#### 7、删除本地分支
```
git branch -D $local_branch_name
```

#### 8、删除远端分支
```
git push origin --delete $remote_branch_name
```

### Linux下安装git

```
yum -y install git
```



## 遇到过的问题

### 1、git push总是需要输密码的问题

```
现象：使用git push往github推代码的时候总是提示我输入密码，我明明已经把我的公钥拷贝到GitHub了，输错了就更烦人了
```

![1640822162457](E:\MyStudy\LinuxStudy\pictures\1640822162457.png)

```
解决方法：找到当前你本地代码库的.git文件夹，里面有一个config文件，打开之后在remote origin选项中可以看到一个url，这个url如果是http的地址，就会去校验你的密码，没有走ssh这条线路，所以需要把这个地方的地址换成ssh的地址，就解决了需要输入密码的问题
```

![1640822599033](E:\MyStudy\LinuxStudy\pictures\1640822599033.png)



https://www.cnblogs.com/tangxuliang/p/11950995.html

### 2、用git push的时候出现了一堆警告，如下
```
warning: push.default is unset; its implicit value has changed in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the traditional behavior, use:

  git config --global push.default matching

To squelch this message and adopt the new behavior now, use:

  git config --global push.default simple

When push.default is set to 'matching', git will push local branches
to the remote branches that already exist with the same name.

Since Git 2.0, Git defaults to the more conservative 'simple'
behavior, which only pushes the current branch to the corresponding
remote branch that 'git pull' uses to update the current branch.

See 'git help config' and search for 'push.default' for further information.
(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode
'current' instead of 'simple' if you sometimes use older versions of Git)

```
当 push.default 的值设置成 ‘matching’ ，git 将会推送所有本地已存在的同名分支到远程仓库
从 Git 2.0 开始，git 采用更加保守的值’simple’，只会推送当前分支到相应的远程仓库，’git pull’ 也将值更新当前分支。

警告：push.default （默认push）未设置；在Git 2.0 中，push.default 的值从‘matching’改为‘simple’了。消除此警告并保留以前的习惯，输入：
git config --global push.default matching
消除此警告并采用新的设置值，输入：git config --global push.default simple

### 3、分支误删除了如何恢复
1、git log -g 找回之前提交的commit id

2、git branch recover_branch[新分支] commit_id  根据commit将文件恢复到新分支











