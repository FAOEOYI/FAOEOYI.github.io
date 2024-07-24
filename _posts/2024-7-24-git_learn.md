---
layout : post
title : "git学习笔记"
author : "法厄异"
header-style: text
catalog : true
tags:
     git
---
# git学习笔记

## 一、基本使用

#### 1、首先安装git到Ubuntu中，这只需要一条简单的命令：

```shell
sudo apt install git
```

#### 2、git初始化

```shell
git init
```

#### 3、暂存(add)和提交(commit)

```shell
git add .	
```

```shell
git commit -m "备注信息"
```

#### 4、用 git status 命令查看当前状态

```shell
git status
```

#### 5、内容回退到最近一次保留的版本：

```shell
git checkout -- 文件名
```

#### 6、通过 git diff 来查看修改的信息

```shell
git diff
```

```shell
gec@ubuntu:~/dir $ git diff
diff --git a/poem.txt b/poem.txt
index 5410c1e..a28e948 100644
--- a/poem.txt
+++ b/poem.txt
@@ -1,2 +1,4 @@
 一去二三里
-茅舍四五家
+烟村四五家
+亭台六七座
+八九十枝花
```

其中a/poem.txt指的是工作区中我们直接修改的文本，b/poem.txt指的是暂存区中对应的文本信息，可以看到，diff子命令告知了我们它们两者之间的差异。命令 git diff 可以用来检测不同区域不同文件的差异信息，他的用法有很多种，比如常用的有这些：

- ① git diff：检测工作区与暂存区信息的差异
- ② git diff xxx：检测工作区与暂存区中关于文件xxx信息的差异
- ③ git diff --cached：检测暂存区与当前分支最新版信息的差异
- ④ git diff --cached xxx：检测暂存区与当前分支最新版中关于文件xxx信息的差异
- ⑤ git diff HEAD：检测工作区与当前分支最新版信息的差异
- ⑥ git diff HEAD xxx：检测工作区与当前分支最新版中关于文件xxx信息的差异
- ⑦ git diff SHA1 SHA2：检测两个版本之间的差异

下图可以一目了然地对这些命令的作用做个总结。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAb15TAACarnjc0Vk049.png?token=null&ts=null)

#### 7、使用命令 git log 来查看所有commit过的版本变更的详细记录

```shell
git log		
```

在多次修改了文档信息之后，git log显示出来的列表可能会相当长，可以使用下面的命令来简化log的输出信息：

```shell
git log --pretty=oneline
```

#### 8、提交与撤销

回退到上一个版本

```shell
git reset --hard HEAD^
```

其中，HEAD是当前分支的最新版本，HEAD^ 指的是上一版本;

以此类推，HEAD^^ 就是上上一版，有多少个上尖号就回溯多少次上一版本;

但是太多的话算起来就会比较容易出错，因此回溯到上12个版本可以写成 HEAD~12。

#### 9、使用 git reflog查看版本编号并使用编号回溯

在git 中做过的行为，统统都会被记录下来,如何查看所有的改动？

```shell
gec@ubuntu:~/dir $ git reflog
becf665 HEAD@{0}: reset: moving to HEAD^
cdce558 HEAD@{1}: commit: 一首漂亮的古诗
becf665 HEAD@{2}: commit (initial): 写了前两句
```

从上面的命令结果找到想要的版本号是：cdce558 ，因此再次调用 reset来将版本变迁到cdce558：

```shell
gec@ubuntu:~/dir $ git reset --hard cdce558
```

下图总结了在工作区、暂存区和版本节点之间递交和撤销数据的不同命令的关系。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAGJqgAABPQJmFA-g002.png?token=null&ts=null)

## 二、分支管理

#### 1、分支管理

​		git的分布式特性已经让我们欢呼不已，但git的魅力远不止这些，git还有一个大招，那就是神奇的分支。上面提到，每次commit都会将暂存区中的最新修改信息递交到版本库中，从而形成一系列不同时间点的版本节点，形成一个版本历史，这样的一个历史线条，称之为一个分支。git允许并且鼓励一个项目有多个分支，每创建一个分支，相当于将版本演进放到一个单独的世界，不受外界的干扰，独自演化，到恰当的时候可以融合。这非常适合于多人协作的软件开发情景，如下图所示。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAQa1CAACSDUkYuCw203.png?token=null&ts=null)

#### 2、分支的使用场景

继续探讨分支之前，先来总结分支的基本使用场景：

1. 多人协作开发时，每个小分队拥有自己的分支，开发完一个阶段之后融合进主版本
2. 为稳定的主版本加入新功能时，创建新分支进行Develop，测试成功后融入主版本
3. 为已发布的主版本进行Debug时，创建新分支进行修复，成功之后融入主版本
4. 将不同软件的版本库作为分支进行融合，这正是开源软件协作的基石

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAJzceAAHKNA_u5Ws605.png?token=null&ts=null)


回忆之前两个小节的内容，master是git在初始化的时候默认产生的分支，而HEAD是当前分支的最新版本，实际上master是一个指针，它指向主分支的历史线的最新版本节点，而HEAD是一个指向master的指针，容易想到，如果当前仓库有多个版本分支，比如Develop分支，那么HEAD可以随时切换到Develop。它们的关系如下图所示。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJKAHJapAACOsuPsGOQ248.png?token=null&ts=null)


#### **3. 分支的创建和切换**

下面来用一个例子说明分支与融合的整个基本操作过程，假设现在我要写一篇文稿，写完使用git保存最新版本。首先初始化一个git仓库，创建一个git.txt并将其commit到当前默认的master分支中，然后使用branch命令查看当前所在的分支。

```
gec@ubuntu:~/dir $ cat -n git.txt 
1Git is a free and open source
gec@ubuntu:~/dir $ git add .
gec@ubuntu:~/dir $ git commit -m "A"

gec@ubuntu:~/test$ git branch 
* master
```

可以看到当前分支是master，这是git初始化时自动创建的分支，随着给文档继续添加内容，不断commit，就会形成一个历史版本时间线，假设每次commit的时候使用A、B、C、D来简单描述当前的节点，那么经过几次commit之后的结果大概会如下左图所示。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJKARQFsAAIClczTnT4293.png?token=null&ts=null)
主分支的逐步演进          新建分支dev并且换分支

接着，我们打算新建一条分支dev，用来开发一个新的功能，当测试完成之后再融入主版本分支，如上右图所示。

```shell
gec@ubuntu:~/dir $ git checkout -b dev		——(1)
Switched to a new branch 'dev'

gec@ubuntu:~/dir $ git branch dev			——(2)
gec@ubuntu:~/dir $ git checkout dev			——(3)
Switched to branch 'dev'
```

上面命令(1)和命令(2)+(3)的效果是等价的，都意味着先创建一个分支dev，然后切换到分支dev，此时使用branch命令再来查看当前所在的分支，会发现已经切换到dev下了。

```shell
gec@ubuntu:~/dir $ git branch 
* dev
  master
```

接着，在dev上对当前仓库做出修改，并将修改后的版本commint到版本库中

```shell
gec@ubuntu:~/dir $ cat -n git.txt 
     1	Git is a free and open source
     2	distributed version control system
     3	designed to handle everything
     4	from small to very large projects
     5	with speed and efficiency
gec@ubuntu:~/dir $ git add .
gec@ubuntu:~/dir $ git commit -m "E"
```

新的版本E是分支dev上的最新节点（而不是master）。此时各分支的关系如下图所示。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJKALuYAAADAl_CIoH0453.png?token=null&ts=null)
新分支dev上的版本迭代

#### **4. 分支的融合**

注意，此时我们的工作在dev分支上，这上面所做的一切均不会对原有的master分支代码版本有任何影响，因此master作为在工程开发中发布给用户使用的稳定版本，不会因为软件的更新迭代而受影响。现在假设新的功能开发并测试完毕，可以融入主版本，所需要的操作非常简单：

```shell
gec@ubuntu:~/dir $ git checkout master 		——(1)
Switched to branch 'master'
gec@ubuntu:~/dir $ git merge dev			——(2)
Updating 21dbcb1..89c06e0
Fast-forward
 git.txt | 1 +
 1 file changed, 1 insertion(+)
gec@ubuntu:~/dir $ git branch -d dev 		——(3)
Deleted branch dev (was 89c06e0).
```

- 命令(1)：切换到master分支
- 命令(2)：将分支dev融合到当前分支（即master分支）
- 命令(3)：将分支dev删除

上述操作步骤，就是典型的开发流程——在一个对外不可见的分支上，做完了开发与测试，或者修复了Bug之后，将新分支融合进主版本，然后将分支删除。
当然，上述命令(2)的正常执行是有条件的，那就是当前的master分支的最新版内容，与指定的dev分支上最新版的内容没有冲突，可以融合。

#### **5. 分支的冲突与解决**

来考虑另一种很常见的情况，那就是张三在master分支（这只是举例，一般master是不允许做任何修改的！）上做了修改，而李四再次创建dev分支并在上面做了不同的修改，此时张三再想把这两条分支融合，就必然会产生冲突。首先是张三在master上的修改：

```shell
张三@ubuntu:~/dir $ git branch 
  dev
* master
张三@ubuntu:~/dir $ cat -n git.txt 
     1	Git is a free and open source
     2	distributed version control system
     3	designed to handle everything
     4	from small to very large projects
     5	with speed and efficiency
     6	git hard to learn
```



而与此同时，李四在dev上也在文档的末尾添加了一行：

```shell
李四@ubuntu:~/dir $ git branch 
* dev
  master
李四@ubuntu:~/test$ cat -n git.txt 
     1	Git is a free and open source
     2	distributed version control system
     3	designed to handle everything
     4	from small to very large projects
     5	with speed and efficiency
     6	git easy to learn
```



然后，当张三和要把李四的dev分支融合的时候，Boom！爆炸了！两个不一样的修改导致git无所适从。以上情形读者可以使用同一个用户来模拟。

```shell
张三@ubuntu:~/dir $ git merge dev
Auto-merging git.txt
CONFLICT (content): Merge conflict in git.txt
Automatic merge failed; fix conflicts and then commit the result.
```



此时的冲突如下图所示，由两个不同的修改者沿着两条分支到达不同的版本节点，由此带来了冲突。当执行了merge命令之后，git将会将差异信息直接写入相关文件内，并且锁定当前分支，无法切换到别的分支，直到手动将差异统一为止。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJKACPbnAAB0CdKxCCA682.png?token=null&ts=null)


查看当前的git.txt的内容，将会看到两分支版本的差异：

```shell
gec@ubuntu:~/dir $ cat -n git.txt
     1	Git is a free and open source
     2	distributed version control system
     3	designed to handle everything
     4	from small to very large projects
     5	with speed and efficiency
     6	<<<<<<< HEAD
     7	git is hard to learn
     8	=======
     9	git is easy to learn
    10	>>>>>>> dev
```



接下来把内容修改好，然后再将最终修改版添加并commit到版本库：

```shell
gec@ubuntu:~/dir $ git add .
gec@ubuntu:~/dir $ git commit -m "F"
[master 0e445b2] F

gec@ubuntu:~/dir $ git log --graph --pretty=oneline --abbrev-commit 
*   0e445b2 F
|\  
| * 4b31417 E
* | 3893e70 E
|/  
* 21dbcb1 D
* ca051de C
* 7b03b81 B
* 9d45934 A
* 1fd2a77 initialize
```



从上述 git log 命令的输出看到，经过手工修改并commit之后，两个分支成功融合为一个版本了。可见，经过分支和融合就可以实现多路开发，这个功能十分强大，每个人都可以有自己的分支，团队合作的分支看起来差不多是下图的样子。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJKAKefuAADxpsklT2I208.png?token=null&ts=null)

## 三、远程仓库

#### **1. 分布式版本管理**

上一节介绍了版本管理系统的最一般的功能，除此之外，别忘了git有别于传统CVS和SNV的一点，是其分布式的特点。传统版本控制系统都是集中式的，有一个中央服务器，上面存放了唯一的版本历史，当我们用自己的电脑进行开发时，需要从中央服务器下载最新的版本，更新数据之后又要推送到服务器，如图1-35所示。如果跟同事的修改有冲突，还需要消耗更多的带宽来处理冲突，这要是遇到网络比较糟糕的环境，一次蜗牛般缓慢的版本递交过程就会让人崩溃，这是集中式控制的最大弊端。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAD_vlAACLcGTQC_U937.png?token=null&ts=null)
集中式版本管理

分布式系统，顾名思义就是每一台电脑都保存了完整的历史版本信息，不存在任何中央服务器，数据被分散到所有的电脑中，如图1-36所示。这样能一方面能大大增加数据的安全性，另一方面能摆脱网络的带宽的束缚。既然每个人都有自己完整的版本库，那多人协作怎么进行呢？比如对于一个项目，我在家修改了模块A，同事小张也修改了模块A，这样的话我们两个人可以互相将自己的修改推送给对方，让对方查看之间的差异，比如我觉得小张的修改比我的要好，那么我就可以将小张的版本融合到我的本地版本库中。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAN_a3AAC2tIpfcQw467.png?token=null&ts=null)
分布式版本管理

#### **2. 版本管理服务器**

实际上我们很少会在两台电脑之间直接推送彼此的修改，一方面是因为不同电脑可能处于不同的网络无法直接访问，另一方面多人协作可能涉及远远超过两个人的情形，因此在实际生产情境下，分布式系统也往往需要一台能24小时开机、可被外网访问的服务器，但这个服务器仅仅用来充当多人协作的版本中转站而已，大家离开了它也能照样工作，这台服务器被用来协调多人开发的版本分支和融合问题。

许多公司都会在公司内部搭建一台git服务器，用来接收和融合各个开发人员递交的版本，人们在离开公司的时候所做的开发工作，可以在任意方便的时候推送到git服务器，不方便的时候可以直接保存在本地电脑中，没有时刻要联网递交修改的要求。当然，git的优势绝不仅限于此，git的分支管理是它的另一门必杀技，后续会讲解。

从以上描述容易想到，应该会有很多公司、组织或者个人无法或者不想搭建一台git服务器，但是又想要享受随时随地可以将本地版本库推送到一个可以被互联网上的其他人很方便访问的公共服务器上，于是https://gitee.com就诞生了，从名字看得出来这就是一个提供git仓库托管服务的平台，目前gitee上已经是华语世界最大的开源软件集聚区（对标美国的github，github容易被墙且访问速度缓慢，推荐支持国产gitee），众多优秀的开源软件，基本上在gitee上都能找到，而你只需要在gitee上注册个账号，就可以下载并参与开发，并且拥有自己的git远程仓库。

#### **3. 新建gitee账户**

以笔者在gitee上的账户vincent040为例，如果目前有一个开发中的项目，想跟其他同事一起进一步完善，那大致的过程是：

1. 在gitee上建立好版本仓库
2. 将我电脑上的本地仓库推送到gitee上
3. 每个同事将gitee上版本代码fork到自己的本地电脑中
4. 每个同事做了某些修改后，推送到gitee中待审核
5. 审核通过，则新的修改将会被融合进新的版本中，不断重复后两步

下面来逐步展开以上的操作。首先，到gitee.com上注册一个账号，并创建一个新的版本仓库，如下图所示。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAGeDGAABJhxO9OYc042.png?token=null&ts=null)
创建新的版本仓库repository

接着，在弹出的表单中填入项目的名称，以及一个简单明了的概要。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJKAaWONAABu9Ar4umA570.png?token=null&ts=null)
填写项目名称和描述

#### **4. 推送仓库到gitee**

创建好之后，gitee会提示你，在命令行可以直接将一个已有的仓库推送（push）到gitee上，假设我要将上面写的那首诗作为版本仓库推送到gitee，并让别人一起修改，那么我就可以将代码推送到这个justForFun项目中去，命令如下：

```shell
$ git remote add origin https://gitee.com/vincent040/justForFun.git
$ git push -u origin master 
Username for 'https://gitee.com': vincent040
Password for 'https://vincent040@gitee.com': ******
Counting objects: 6, done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (6/6), 556 bytes | 0 bytes/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To https://gitee.com/vincent040/justForFun.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```



其中，第一条remote语句为在本地仓库添加一个关联的远程仓库，origin作为在本地访问远程仓库时使用的名字，是git默认的名字。第二条语句就是将本地仓库推送并同步到远程，这个过程当然需要联网。完成之后在gitee上刷新一下可以看到本地的版本已经被推送成功，如下图所示。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAMANUAABEXEU6oPs860.png?token=null&ts=null)
将本地仓库推送到gitee

#### **5. 参与项目开发**

##### **5.1 复刻gitee上的开源项目**

此时，假设有某位同事Jack对我这首古诗感兴趣，并想要参与创作，那么他可以随时在gitee上复刻（Fork）该项目，点击下图最右边的按钮Fork，即可将这个项目复刻到Jack自己的远程版本库上。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAR409AAAH0GeUgjg051.png?token=null&ts=null)
Fork复刻项目

接着Jack需要将他远程版本库上的这个项目克隆到他自己的电脑上，那样才方便编辑和开发，在Jack的电脑中，可以使用以下命令将gitee上的项目克隆到本地：

```shell
Jack@ubuntu:~/$ git clone https://gitee.com/Jack/justForFun
Cloning into 'justForFun'...
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 6 (delta 0), reused 6 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
Checking connectivity... done.
```



##### **5.2 对原项目发起PR**

Jack重新对诗文进行编撰之后，觉得很满意，希望我接纳他的修改，于是他将修改之后的诗文push到他自己的远程版本库gitee.com/Jack/justForFun上，并且对我发起了拉取请求。

```shell
Jack@ubuntu:~/justForFun$ git add .
Jack@ubuntu:~/justForFun$ git commit -m "做了一些不可思议的修改"
[master 6583dd8] 做了一些不可思议的修改
 1 file changed, 3 insertions(+), 1 deletion(-)
Jack@ubuntu:~/justForFun$ git push
```



发起拉取请求（PullRequest，即PR）的含义，就是Jack希望项目原主看到他所做的修改，并且通过比对接纳他的版本，当然也可以不接纳。Jack给原主发送拉取请求的做法很简单，只需要在gitee相应项目中点击Pull request就可以了，在实际操作前，还可以再次点击旁边的Compare按钮来仔细确认一遍所做的修改，如图1-41所示。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGASeVlAABRizr5Trw704.png?token=null&ts=null)
发起Pull request

##### **5.3 对PR做进一步说明**

按下按钮Pull request之后，Jack可以对自己的修改做更多解释：

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAGHxvAAA5BXaJxdE535.png?token=null&ts=null)
对PR做解释

这些信息将会被gitee一一记录下来，其他的拉取了该项目的成员都可以看得见，并且都可以发表意见，所有的讨论都会一目了然地呈现在相应的页面中，让后来的人们可以马上了解项目的进度和讨论的过程。

然后，Jack就可以等待作者的回应。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGACfXYAAB4W-n5Ev0815.png?token=null&ts=null)
等待作者回应

#### **6. 接收并评审PR**

另一边，我此时应该就收到了Jack的拉取请求了，让我来看看他究竟对我的诗文做了什么修改。通过点击项目justForFun的Pull request标签，就可以看到是谁发起的拉取请求，他总共做过几次提交，对什么文件做了什么修改，都可以很清楚地看到，同时可以参与讨论，如下列截图所示

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGADB2VAABtTv4z9rs923.png?token=null&ts=null)
查看待处理的PR

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAYoVAAAAqotM2Ido318.png?token=null&ts=null)
查看Jack做了哪些提交

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJGAV7qQAAAqbvL0_-A609.png?token=null&ts=null)
查看Jack具体的修改

如果我对Jack的修改很满意，那么就可以将他的版本融合（Merge）到我当前的版本中，点击图1-42左下角的“Merge Pull request”即可融合代码了。

![img](http://edu.yueqian.com.cn/group1/M00/00/97/rBJlJmRbVJKAfulwAABIqTEmrA8008.png?token=null&ts=null)
代码评审
以前，使用集中式的CVS和SNV是一种煎熬，现在，使用git和gitee来进行团队开发变成了一种乐趣！
