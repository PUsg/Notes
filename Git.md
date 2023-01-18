# Git



## Git介绍  

分布式版本控制及工具 vs 集中式版本控制工具

集中式：所有人都通过一个服务器管理文件 可以轻松的掌握每个访问者的权限、

分布式：断网的情况下也可以进行开发

Git[易于学习](https://git-scm.com/doc)，[占用空间很小，性能快如闪电](https://git-scm.com/about/small-and-fast)。 它超越了Subversion，CVS，Perforce和ClearCase等SCM工具。 具有[廉价本地分支](https://git-scm.com/about/branching-and-merging)等功能， 方便[的暂存区域](https://git-scm.com/about/staging-area)和[多个工作流程](https://git-scm.com/about/distributed)。

### 版本控制

最重要的是可以记录文件修改的历史记录

### 工作机制

![image-20221202203905659](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20221202203905659.png)

工作区和暂存区的代码可以删掉

进入本地库的代码不能够删掉



### Git的代码托管中心

是基于网络服务器的远程代码仓库，一般我们简单称为远程库

局域网：**GitLab** 

互联网：**GitHub** **Gitee**





## Git命令

### 常用命令

设置用户签名

```git
git config --global user.name
```

```
git config --global user.email
```

```
//初始化目录
$ git init
Initialized empty Git repository in D:/Git-Space/git-demo/.git/
```

```
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

```
//提交暂存区
$ git add hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
```

```
//从暂存区删除 但是不删本地
$ git rm --cached hello.txt
rm 'hello.txt'

```

```git
// 提交本地库
$ git commit -m "first commit" hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
[master (root-commit) 22b60b5] first commit
 1 file changed, 11 insertions(+)
 create mode 100644 hello.txt

```





### 历史版本

```
//查看引用日志信息
$ git reflog
22b60b5 (HEAD -> master) HEAD@{0}: commit (initial): first commit
```

```
//查看详细日志信息
$ git log
commit 22b60b506380db10b6ea3834f8d80eefa8ba53ab (HEAD -> master)
Author: Puc <2353559384@qq.com>
Date:   Sat Dec 3 10:01:10 2022 +0800

    first commit

```



### 版本穿梭***

```
// 
$ git reset --hard 22b60b5 // 版本号 用 git reflog 查看
HEAD is now at 22b60b5 first commit
```







## Git分支

版本控制过程中，同时推进多个任务，为每个任务 ，我们就可以创建单独的分支

底层也是指针的引用

同时并行推进多个功能开发 提高开发效率

分支在开发过程中 如果某一个分支开发失败 不会对其他分支有任何影响





### 分支的操作

```
//查看分支
$ git branch -v
* master 22b60b5 first commit
```



```
//创建分支
$ git branch hot-fix
```



```
//切换分支
$ git checkout hot-fix
Switched to branch 'hot-fix'
```



```
//合并分支 （正常合并）
$ git merge hot-fix
Updating 22b60b5..3ad201f
Fast-forward
 hello.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)

```

```
//冲突合并
// 两个分支的同一个文件的同一个位置 有不同的修改 必须人为决定保留那个代码
$ git merge hot-fix
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.

垃圾电脑@LAPTOP-J2LTTLRG MINGW64 /d/Git-Space/git-demo (master|MERGING)

//$ git commit -m "merge test" hello.txt
//fatal: cannot do a partial commit during a merge 报错


$ git commit -m "merge test"  // 不能带文件名
[master 88b35a3] merge test

// 注意 合并之后 只有合并的那个分支会改变  被合并的那个分支不会改
// master 会改 hot-fix 不会

```



## Git团队协作机制

### 团队内协作

### 跨团队协作



## IDEA集成Git

配置忽略文件















# GitHub

全球最大的同性交友网站 技术宅男的天堂



## 创建远程库





## 代码推送 Push

```
$ git push git-demo master  //最小单位是分支
						  //网络要求高 
						  
SSL certificate problem: unable to get local issuer certificate //报错 这个问题是由于没有配置信任的服务器HTTPS验证。默认，cURL被设为不信任任何CAs，就是说，它不信任任何服务器验证。			  
$ git config --global http.sslVerify false //解决报错
						  
						  
```



## 代码拉取 Pull

```
//拉取远程库到本地库
$ git pull git-demo master

$ git status
On branch master
nothing to commit, working tree clean // 本地库是干净的，说明pull操作会自动提交本地库

```





## 代码克隆 Clone

```
clone: 1.拉取代码 2.初始化本地仓库 3.创建别名
```



## SSH免密登录







# Gitee码云





## 创建远程库

## IDEA集成Gitee

## 码云连接Github 







# GitLab

## GitLab服务器的搭建和部署

## Idea集成GitLab