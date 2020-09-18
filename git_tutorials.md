[TOC]
# Github使用教程
强烈推荐
- [高质量的Git中文教程](https://github.com/geeeeeeeeek/git-recipes/wiki)
- [Git Documentations](https://git-scm.com/book/zh/v2)

## 基本指令
### 对现有的某个项目管理 ``git init``
某个项目开始使用Git管理：在项目所在目录执行：
```
git init
```
初始化后，在当前目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。不过目前，仅仅是按照既有的结构框架初始化好了里边所有的文件和目录，但我们还没有开始跟踪管理项目中的任何一个文件。

如果当前目录下有几个文件想要纳入版本控制，需要先用 ```git add``` 命令告诉 Git 开始对这些文件进行跟踪，然后提交：
```
git add *.c
git add README
git commit -m 'initial project version'
```
### 从现有仓库克隆``git clone``
如果想对某个开源项目出一份力，可以先把该项目的 Git 仓库复制一份出来，这就需要用到```git clone```命令。如果你熟悉其他的 VCS 比如 Subversion，你可能已经注意到这里使用的是 clone 而不是```git checkout```。这是个非常重要的差别，Git收取的是项目历史的所有数据（每一个文件的每一个版本），服务器上有的数据克隆之后本地也都有了。实际上，即便服务器的磁盘发生故障，用任何一个克隆出来的客户端都可以重建服务器上的仓库，回到当初克隆时的状态（虽然可能会丢失某些服务器端的挂钩设置，但所有版本的数据仍旧还在）。
克隆仓库的命令格式为 ```git clone [url]```。比如，要克隆 Ruby 语言的 Git 代码仓库 Grit，可以用下面的命令：
```
git clone git://github.com/schacon/grit.git
```
这会在当前目录下创建一个名为 **grit** 的目录，其中包含一个 .git 的目录，用于保存下载下来的所有版本记录，然后从中取出最新版本的文件拷贝。如果进入这个新建的 grit 目录，你会看到项目中的所有文件已经在里边了，准备好后续的开发和使用。如果希望在克隆的时候，自己定义要新建的项目目录名称，可以在上面的命令末尾指定新的名字：
```
git clone git://github.com/schacon/grit.git mygrit
```
唯一的差别就是，现在新建的目录成了 **mygrit**，其他的都和上边的一样

### 检查当前项目文件状态 ```git status ```
要确定哪些文件当前处于什么状态，可以用 ```git status``` 命令。如果在克隆仓库之后立即执行此命令，会看到类似这样的输出：
```
git status 
# On branch master
    nothing to commit (working directory clean)
```
修改或者添加新文件后再用此命令会出现另外的显示。

### 查看日志``git log``
git log 命令可以显示所有提交过的版本信息
- 推荐[Git log 高级用法](https://github.com/geeeeeeeeek/git-recipes/wiki/5.3-Git-log-%E9%AB%98%E7%BA%A7%E7%94%A8%E6%B3%95)
- ``git log --oneline``标记把每一个提交压缩到了一行中。它默认只显示提交ID和提交信息的第一行。

- ``git reflog``查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

- ``git log --graph``绘制一个ASCII图像来展示提交历史的分支结构。它经常和--oneline和--decorate两个选项一起使用，这样会更容易查看哪个提交属于哪个分支:
```
git log --graph --oneline --decorate
```

- 更多相关
[git-log过滤merge信息](#git-log过滤merge信息)

### 分支管理 `` git branch``

>有这么一种情形：在你编写代码的时候，总有那么一份代码是你心里调试好的“完美”的、“没有污染”的。而后面你所做的工作都希望是在这份代码的基础上不断砌砖，这就相当于github上的**branch**的概念了。完美的部分可以叫做**主分支**，而后来的砌砖便可以叫做分支。

查看已经存在的分支：
```
git branch
```

在PC本地新建一个分支：
```
git branch <branch name>
# 推送到远程
git push origin [local branch name]:[remote branch name]
```

查看所有远程分支：
```
git branch -r
```
切换分支：
```
git checkout <branch name>

```
拉取远程分支并创建本地分支
```
#S1:使用该方式会在本地新建分支x，并自动切换到该本地分支
git checkout -b [local branch name] origin/[remote branch name]

#S2:使用该方式会在本地新建分支x，但是不会自动切换到该本地分支x，需要手动checkout
git fetch origin [remote branch name]:[local branch name]
```

删除一个本地分支：
```
git branch -d <branch name>
```
删除一个远程分支：
```
git push origin :<remote branch name>
```
Rename branch
```
git branch -m [old_branch_name] [new_branch_name]
```
### 添加上游仓库 ``git remote``
```
git remote add <name> <url>
```

### 选择部分commit ``git cherry-pick``
[git cherry-pick 教程s](http://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html)

### 分支衍合 ``git rebase``
[Git分支rebase详解](https://blog.csdn.net/endlu/article/details/51605861)
> 如果把衍合当成一种在推送之前清理提交历史的手段，而且仅仅衍合那些尚未公开的提交对象，就没问题。如果衍合那些已经公开的提交对象，并且已经有人基于这些提交对象开展了后续开发工作的话，就会出现叫人沮丧的麻烦。

[彻底搞懂git rebase](https://www.jianshu.com/p/f080912c9cc5)

### 版本恢复命令 ``git reset``
不小心git add了不希望add的文件，可以使用git reset撤回。具体见针对场景的教程

### 打标签 ``git tag``

> 我们可以创建一个tag来指向软件开发中的一个关键时期，比如版本号更新的时候可以建一个“v2.0”、“v3.1”之类的标签，这样在以后回顾的时候会比较方便。tag的使用很简单，主要操作有：查看tag、创建tag、验证tag以及共享tag.

Git 中的tag指向一次commit的id，通常用来给开发分支做一个标记，如标记一个版本号。

打标签:
```
git tag -a v1.01 -m "Relase version 1.01"
#git tag 是打标签的命令，-a 是添加标签，其后要跟新标签号，-m 及后面的字符串是对该标签的注释。
```

提交标签到远程仓库:
```
git push origin -tags
#就像git push origin master 把本地修改提交到远程仓库一样，-tags可以把本地的打的标签全部提交到远程仓库。
```

删除标签:
```
git tag -d v1.01
#-d 表示删除，后面跟要删除的tag名字
```

删除远程标签:
```
git push origin :refs/tags/v1.01
#就像git push origin :branch_1 可以删除远程仓库的分支branch_1一样， 冒号前为空表示删除远程仓库的tag。
```
查看标签:
```
git tag
#或者
git tag -l
```

### 暂存当前正在进行的工作 ``git stash``&``git stash pop``
[git stash的详细讲解](https://www.jianshu.com/p/14afc9916dcb)
[git stash 用法总结和注意点](https://www.cnblogs.com/zndxall/archive/2018/09/04/9586088.html)

> git stash 可用来暂存当前正在进行的工作， 比如想pull 最新代码， 又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作。


基础命令：
```
git stash
<do some work>
git stash pop
```

进阶：
```
Git stash save "work in progress for foo feature"
```

>当你多次使用’git stash’命令后，你的栈里将充满了未提交的代码，这时候你会对将哪个版本应用回来有些困惑，

```
git stash list
```
>命令可以将当前的Git栈信息打印出来，你只需要将找到对应的版本号，例如使用’git stash apply stash@{1}’就可以将你指定版本号为stash@{1}的工作取出来，当你将所有的栈都应用回来的时候，可以使用’git stash clear’来将栈清空

```
git stash          # save uncommitted changes
# pull, edit, etc.
git stash list     # list stashed changes in this git
git show stash@{0} # see the last stash 
git stash pop      # apply last stash and remove it from the list

git stash --help   # for more info
```

## 针对各种场景需求的教程
### 记录每次更新到仓库
现在我们手上已经有了一个真实项目的Git仓库，并从这个仓库中取出了所有文件的工作拷贝。接下来，对这些文件作些修改，在完成了一个阶段的目标之后，提交本次更新到仓库。

工作目录下面的所有文件都不外乎这**两种状态**：***已跟踪或未跟踪***。已跟踪的文件是指本来就被纳入版本控制管理的文件，在上次快照中有它们的记录，工作一段时间后，它们的状态可能是未更新，已修改或者已放入暂存区。而所有其他文件都属于未跟踪文件。它们既没有上次更新时的快照，也不在当前的暂存区域。初次克隆某个仓库时，工作目录中的所有文件都属于已跟踪文件，且状态为未修改。

在编辑过某些文件之后，Git将这些文件标为已修改。我们逐步把这些修改过的文件放到暂存区域，直到最后一次性提交所有这些暂存起来的文件，如此重复。所以使用 Git 时的文件状态变化周期如图 2-1 所示。
![](http://git.oschina.net/progit/figures/18333fig0201-tn.png)

### github免密ssh管理
单账号参考：[Git安装教程(Windows) 以及连接Github](https://zhuanlan.zhihu.com/p/114068278?from_voters_page=true)
多帐号参考：[Windows下Git多账号配置，同一电脑多个ssh-key的管理](https://www.cnblogs.com/popfisher/p/5731232.html)
需求说明：存在多个SSH密匙，希望添加github免密
步骤：
- 生成github.com对应的私钥公钥
```
ssh-keygen -t rsa -C <github_email>
```
然后按提示输入保存文件的文件名（不要使用默认），如：id_rsa_github
![](https://i.imgur.com/8i8WeNP.png)

- 把上面得到的文件拷贝到git默认访问的.ssh目录(win10在用户目录下，本文C:\Users\liyih\\.ssh)
除了秘钥文件之外，config文件是后面的步骤中手动生成的（没有的话），known_hosts文件是后续自动生产的
![](https://i.imgur.com/WzGqqus.png)

- 复制.pub里的内容上传到[github](https://github.com/settings/keys)
![](https://i.imgur.com/3mruU4W.png)

- 在.ssh目录创建config文件（如果没有话）并完成相关配置(最核心的地方)
```config
# github
Host github.com
  HostName github.com
  IdentityFile C:\\Users\\liyih\\.ssh\\id_rsa_github
  PreferredAuthentications publickey
  User lyh458
```
- 测试
``` ssh
ssh -T git@github.com
```

### 将一个本地项目（非git clone）托管到github
S1:
（在远程仓库为空时可以使用）
```
git init
# Only add one file
git add README.md

# Add all change files
git add .

# 提交项目更新
git commit -m "first commit"

# 建立远程仓库
git remote add origin git://github.com:dengzhaotai/vlc_play.git

# 第一次使用加上了-u参数，是推送内容并关联分支。推送成功后就可以看到远程和本地的内容一模一样，下次只要本地作了提交，不用加“-u”了。
git push -u origin master

```

（在远程仓库为非空时）
```
git pull origin master --allow-unrelated-histories

git push origin master

```

S2: 

先在github网页上手动新新建仓库，然后git clone仓库到本地，再把本地项目复制到该文件夹，再按照下面的操作：
```
git add .
git commit -m "<description>"
git push origin master
```

### 将一个从他人仓库git clone下来的修改后的本地仓库push到自己的远程仓库
如果可以建议避免直接clone，先fork为自己仓库，再从自己的远程仓库git clone。
坚持这样的话，按照如下教程实现：
- 在网页端新建一个同名的空白仓库
- 然后再``git push -u origin``

### 从远程的分支获取最新的版本

#### 使用``git fetch``,然后merge
相当于是从远程获取最新版本到本地，不会自动merge
```   
git fetch origin master
git log -p master..origin/master
git merge origin/master
```
以上命令的含义：
   首先从远程的origin的master主分支下载最新的版本到origin/master分支上
   然后比较本地的master分支和origin/master分支的差别
   最后进行合并

上述过程其实可以用以下更清晰的方式来进行：
 
```  
git fetch origin master:tmp
git diff tmp 
git merge tmp
```

从远程获取最新的版本到本地的test分支上之后再进行比较合并

#### 直接使用``git pull``
相当于是从远程获取最新版本并merge到本地
``` 
git pull origin master
```
上述命令其实相当于git fetch 和 git merge在实际使用中，git fetch更安全一些 因为在merge前，我们可以查看更新情况，然后再决定是否合并.

### github上fork过来的项目，如何与原仓库保持同步
#### 1. 在github网页端直接pull request
#### 2. 本地命令行操作
- 查看现有的远程仓库
```
git remote -v
```
一般结果是只有指向自己仓库

- 添加指向原仓库（常成为“上游仓库”）的upstream
```
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

-再次查看现有的远程仓库
```
git remote -v
```
此时应该多了upstream了

- 获取上游仓库的更新到本地
```
git fetch upstream
```
- 切换到本地仓库的某一分支（如master）
```
git checkout master
```

- 合并原仓库的更新到本地（如将upstream/master的更新合并到本地master）
```
git merge upstream/master
```
倒数第1、2、3条命令合一合并成一条``git pull upstream master``，但是相对来说第一种方法保险点

### 忽略不想commit的文件

参考自：[git忽略特殊文件](https://www.liaoxuefeng.com/wiki/896043488029600/900004590234208)

- Git工作区的根目录下新建 **.gitignore** 文件

- 把要忽略的文件名填进去，Git就会自动忽略这些文件。不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

- 最后一步就是把.gitignore也提交到Git，就完成了！

### 通过命令新建一个仓库
```
echo "# ConExt" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/lyh458/ConExt.git
git push -u origin master
```
### push现有的仓库到自己的github
```
git remote add origin https://github.com/lyh458/ConExt.git
git push -u origin master
```

### 如何删除已经提交到远程仓库的commit

- 查看Git提交记录
```git
git log
```

- 找到需要回滚到的提交点，复制它的hash值
```git
git reset --hard <commit_id>
```

- 将当前指向的head推到git
```git
git push origin HEAD --force
```

### 只pull request部分commit(或者分支合并)
pull request可以指定分支,你自己的代码可以创建一个分支(以dev分支为例),需要pull request的代码直接放到master分支或其他分支（以master分支为例）。有两种实现方法：

#### 利用``git cherry-pick``实现
   cherry-pick 就是挑选一个我们需要的 commit 进行操作。它可以用于将在其他分支上的 commit 修改，移植到当前的分支，但是移植的只是一个副本，会生成一个**新的commit记录**，具体地：

参考自[如何优雅地pull request](https://juejin.im/post/6844903648208617485#heading-21)

- 目标：将dev分支的commit1和commit2提交到上游仓库
  
- 在dev分之下，查看commit1的hash值，然后复制commit1和commit2的hash值(或分支名)
```
git log
```

- 切换到master分支
```
git checkout master
```

- 选择commit1和commit2
```
git cherry-pich <commit1_hash> <commit2_hash>
```
可能会出现conflict，[解决方法](https://blog.csdn.net/armwind/article/details/51746027)

- push到自己的远端仓库
```
git push origin master
```

- 然后在网页端提交PR

上面的命令将 A 和 B 两个提交应用到当前分支。这会在当前分支生成两个对应的新提交。
如果想要转移一系列的连续提交，可以使用下面的简便语法。
```
git cherry-pick A..B 
```
上面的命令可以转移从 A 到 B 的所有提交。它们必须按照正确的顺序放置：提交 A 必须早于提交 B，否则命令将失败，但不会报错。

注意，使用上面的命令，提交 A 将不会包含在 Cherry pick 中。如果要包含提交 A，可以使用下面的语法。
```
git cherry-pick A^..B 
```

#### 利用``git rebase``实现
参考自[巧用 git rebase 将某一部分 commit 复制到另一个分支](https://www.cnblogs.com/yxhblogs/p/10561879.html)
在我们的实际开发的过程中，我们的项目中会存在多个分支。在某些情况下，可能需要将某一个分支上的 commit 复制到另一个分支上去。
![](https://i.imgur.com/CD1AnDx.png)

就像这张图所描述的这样，将 develop 分支中的 C~E 部分复制到 master 分支中去,即我们需要将最后三个 commit，复制到 master 分支上去。。这时我们就可以用 git rebase 命令来实现了。
```
// startpoint 第一个 commit id, endpoint 最后一个 commit id，branchName 就是目标分支了。
$ git rebase [startpoint] [endpoint] --onto [branchName]
```

执行 git rebase 命令之后，我们发现当前的 HEAD 处于游离状态。
所以我们需要使用 git reset 命令，将 master 所指向的 commit id 设置为当前 HEAD 所指向的 commit id。

### 合并分支
场景需求：将test分支的新增内容合并到master分支(两个分支存在有交叉及无交叉两种情况)，可以使用四种方法实现：
参考自[图解4种git合并分支方法](https://yanhaijing.com/git/2017/07/14/four-method-for-git-merge/)

#### 两个分支没有交叉时
##### ``git merge``采用fast-forward快速合并
待合并的分支在当前分支的下游，也就是说没有分叉时，会发生快速合并，从test分支切换到master分支，然后合并test分支.
```
git checkout master
git merge dev
```
这种方法相当于直接把master分支移动到test分支所在的地方，并移动HEAD指针（不会产生新的commit）。
<!-- ![](https://i.imgur.com/qH4dVsW.gif) -->
![](http://yanhaijing.com/blog/498.gif)
![fast-forward](https://i.imgur.com/Tlbgfzt.png)

##### 使用``git merge --no-ff``强制非快速合并
如果我们不想要快速合并，那么我们可以强制指定为非快速合并（生成新的合并commit,即merge commit），只需加上--no-ff参数
```
git checkout master
git merge --no-ff dev
```
这种合并方法会在master分支上新建一个提交节点，从而完成合并
<!-- ![](https://i.imgur.com/szJCKOW.gif) -->
![](https://yanhaijing.com/blog/499.gif)
![git merge --no-ff](https://i.imgur.com/S28uJJl.png)

##### 使用``git merge --squash``
svn的在合并分支时采用的就是这种方式，squash会在当前分支新建一个提交节点。
```
git checkout master
git merge --squash dev
```
<!-- ![](https://i.imgur.com/DfYYql2.gif) -->
![](https://yanhaijing.com/blog/500.gif)
squash和no-ff非常类似，区别只有一点不会保留对合入分支的引用（对于文件来说，合并效果和前面两种方式是一样的，但是区别在于执行``git merge --squash``后，master分支处于``git add``后的状态，需要再执行``git commmit``，而前面两种方式是执行完后就处于commit完成状态）
![git merge --squash](https://i.imgur.com/N5hJmDb.png)

##### 使用``git rebase``
在分支没有交叉时，采用``git rebase``的方式与采用``git merge``fast forward是完全一样的。

#### 当两个分支存在交叉时

##### 使用``git merge``

###### 当两个分支存在交叉时，使用``git merge``就等同于``git merge --no-ff``，而且很大机率会出现merge conflict。
![分支存在交叉时，git merge产生的conflict](https://i.imgur.com/VhnmGpF.png)

- 按照提示定位到冲突发生的文件并进行修改，所有冲突解决完后，然后``git add .``
![修改前后](https://i.imgur.com/Jbnd7hV.png)

- 再``git commit``，会自动弹出一个merge commit的注释编辑窗口，编辑或者直接保存即可。
![merge conflict注释编辑](https://i.imgur.com/gyZvZAo.png)

- 查看``git log``会发现最后的合并效果
由于git是根据检测行的变化来检测仓库是否发生变化，所以细心可以发现master commit的顺序会根据修改conflict时行内容对应的commit发生调整。
![merge效果](https://i.imgur.com/iKrtELQ.png)
![commit顺序发生变化](https://i.imgur.com/erT2RYg.png)

###### 当两个分支存在交叉时，使用``git merge --squash``
当两个分支存在交叉时，使用``git merge --squash``不会对dev分支交叉部分引用，而是把dev的交叉部分的commit注释合并生产一个新的squash注释。
![squash commit注释编辑窗口](https://i.imgur.com/4Mlv9B0.png)

![git merge --squash最终效果](https://i.imgur.com/eqGnQSS.png)

##### 使用``git rebase``

rebase与merge不同，rebase会将合入分支上超前的节点在待合入分支上重新提交一遍（即假如当前位于dev分支，执行``git rebase master``后，会将dev分支上超前的节点，在master上重新提交一遍，同时dev HEAD的位置也会相应变化），如下图，B1 B2会变为B1’ B2’，看起来会变成线性历史。
![rebase合并](https://i.imgur.com/kzKXrlt.png)

``git rebase``前
![git rebase前](https://i.imgur.com/vT7n5Tq.png)

``git rebase master``后
![git rebase后](https://i.imgur.com/2jpczlL.png)

当分支存在交叉时，使用``git rebase``时也会大概率存在冲突，而且冲突即使在同一个文件中，也得一个一个解决。解决冲突步骤：
- 修改冲突部分
- ``git add/rm <conflicted_files>``
- ``git rebase --continue``
- 如果第三步无效可以执行``git rebase --skip``，或者撤销修改退出rebase回到原来状态``git rebase --abort``。

#### ``git rebase``使用法则

**！！！注意****The Golden Rule of Rebasing rebase****git rebase的黄金法则**：不要在公开的分支上使用``git rebase``，即一旦分支中的提交对象发布到公共仓库，就千万不要对该分支进行衍合操作。
简单来说：一旦你的commit已经推送到远程仓库，并且有其他人已经fork去使用，那么就不要对这些commit执行``git rebase``操作了。

参考自[Git分支Rebase详解](https://blog.csdn.net/endlu/article/details/51605861)
[git rebase成功后如何撤销](#git-rebase成功后如何撤销)

#### 使用``git cherry-pick``

``git cherry-pick``给你自由，想把那个节点merge过来就把那个节点merge过来，其合入的不是分支而是提交节点，所以适用于任何情况。

#### 优缺点对比

merge结果能够体现出时间线，但是rebase会打乱时间线；rebase看起来简洁，但是merge看起来不太简洁

##### ``git merge``

- marge 特点：自动创建一个新的commit，如果合并的时候遇到冲突，仅需要修改后重新commit
- 优点：记录了真实的commit情况，包括每个分支的详情
- 缺点：因为每次merge会自动产生一个merge commit，所以在使用一些git 的GUI tools，特别是commit比较频繁时，看到分支很杂乱。

##### ``git rebase``

- rebase 特点：会合并之前的commit历史
+ 优点：得到更简洁的项目历史，去掉了merge commit
- 缺点：如果合并出现代码问题不容易定位，因为re-write了history

### 合并多个commit

参考[(Git)合并多个commit](https://segmentfault.com/a/1190000007748862)
[彻底搞懂Git Rebase](https://www.jianshu.com/p/f080912c9cc5)
使用 Git 作为版本控制的时候，我们可能会由于各种各样的原因提交了许多临时的 commit，而这些 commit 拼接起来才是完整的任务。造成问题：
不利于代码 review；会造成分支污染。使用``git rebase``可以解决这个问题。
- 获取commit ID
```
git log
```

- 假如需要合并从HEAD版本往前的3个版本，使用：
```
git rebase -i HEAD~3
```
注意：执行该命令所得的排列（从旧到新）与``git log``的排列（从新到旧）相反

- 假如需要合并某个版本（如3a4226b）往后的其他版本（不包含3a4226b,且比它新的版本），使用：
```
git rebase -i 3a4226b
```

- 然后根据提示修改（一般保留最前面的那一个为pick，其他都改为squash），``wq``或者``x``保存退出。

- 修改注释，保存退出。
Git会压缩提交历史，如果有冲突，需要修改，修改的时候要注意，保留最新的历史，不然我们的修改就丢弃了。修改以后要记得敲下面的命令：
   ```
   git add <conflict_files>
   git rebase --continue
   ```

- 推送覆盖远程仓库
```
git push --force
```

- 如果希望合并不相邻的commit，可以使用``git rebase -i <commit ID>``先调整顺序，再按照上面的方法合并。参见[Git调整commit之间顺序](https://www.softwhy.com/article-8639-1.html)

### 修改commit注释

#### 只修改上一次的commit注释
使用``git commmit --amend``指令，然后直接编辑注释，然后保存，最后``git push -f``。

#### 修改历史commit注释
假设要修改倒数第三条commit的注释
- 执行 git 命令, 修改近三次的信息
```
git rebase -i HEAD~3
```

- 将该条信息（因为与``git log``的排列相反，所以此时该条commit应该是位于第一位）前的``pick``改为``edit``或者``e``，保存退出。

- 修改改条commit的注释,保存退出
```
git rebase --amend
```

- 执行继续
```
git rebase --continue
```

- 最后强制push

### ``git rebase``成功后如何撤销

参考自[git rebase 成功之后如何撤销](https://blog.csdn.net/chengde6896383/article/details/83418488)
git rebase 过程中可以使用git --abort/--continue来进行操作，成功之后如何撤销呢？

- 首先执行``git reflog``查看本地记录，找到rebase前的commit ID

- 然后执行``git reset --hard <commit ID>

### ``git log``过滤Merge信息
参考[git log 过滤Merge信息](https://www.jianshu.com/p/11e30cf91ccb)
git log输出包含merge信息。但是，如果开发组总是把上游分支里的更新mege到feature分支，而不是将feature分支rebase到上游分支，就会在代码库中看到非常多的merge信息。
- 使用--no-merges来过滤掉这个merge信息
```
git log --no-merges
```

- 统计merge在log中的总数
```
git log --merges |grep 'Merge branch'|wc -l
```

### 利用``git stash``暂存未commit的修改
场景需求：当你的开发进行到一半，但是代码还不想进行提交，然后需要同步去关联远端代码时。如果你本地的代码和远端代码没有冲突时。可以直接通过git pull解决。但是如果可能发生冲突怎么办.直接git pull会拒绝覆盖当前的修改。

解决方法：使用``git stash``暂存本地代码，然后``git pull``，最后使用``git stash pop``取回暂存的代码
```
git stash
git pull
git stash pop
```
``git stash``详细用法参见[git stash](#暂存当前正在进行的工作-git-stashgit-stash-pop)

当本地（已经commit）和远程仓库都做了不同修改，如何pull远程仓库的内容？参考[git pull --rebase](#使用git-pull---rebase实现当本地已经commit和远程仓库都做了不同修改pull远程仓库的内容)

### 使用``git pull --rebase``实现当本地（已经commit）和远程仓库都做了不同修改，pull远程仓库的内容

场景需求：当本地（已经commit）和远程仓库都做了不同修改，此时如果想push本地的修改到远程仓库，会被rejected。此时可以先pull，然后便可以成功push。但是往往会出现[Merge branch 'master' of ...](#merge-branch-master-of-)的问题。解决方法：
```
git pull --rebase
```
如果pull不产生冲突，会直接rebase，不会产生分支合并操作；如果有冲突则需要手动fix后，自行合并。参考[Git push 时如何避免出现 "Merge branch 'master' of ..."](https://www.cnblogs.com/Sinte-Beuve/p/9195018.html)

- [git log高阶用法](https://www.jianshu.com/p/73f13d2725a8)

### 版本恢复的各种场景
``git add``或者``git commit``或者push了不希望的文件，可以使用相应指令恢复。
参考自[git放弃本地文件修改](https://www.jianshu.com/p/c0f7e4ac14c7)

#### 未使用``git add``缓存代码，使用``git checkout``放弃修改
- 放弃指定文件修改
```
git checkou -- <filename>
```

- 放弃所有文件的修改
```
git checkout .
```
或者
```
git checkout -f
```
此时你修改的文件和删除的文件都会被恢复，但是你新添加的文件不会被删除

- 放弃指定文件夹的修改
```
git checkout <directory>
```
此时指定目录下修改的文件和删除的文件都会被恢复，但是你新添加的文件不会被删除

#### 已使用``git add``缓存代码，未使用``git commit``
- 放弃单个文件修改
```
git reset HEAD <filename>
```

- 放弃所有文件的修改
```
git reset HEAD
```
此命令用来清除 git 对于文件修改的缓存。相当于撤销 git add 命令所在的工作。在使用本命令后，本地的修改并不会消失，而是回到了第一步1. 未使用git add 缓存代码，继续使用用``git checkout -- <filename>``，就可以放弃本地修改

#### 已经用``git commit``提交了代码
- 回退到上一次commit的状态
```
git reset --hard HEAD^
```

- 回退到任意版本
```
git reset --hard <commitID>
```

#### ``git clean``放弃新添加的文件或文件夹
- 放弃 仓库所有 添加
```
git clean –df
```
此时该仓库下所有新添加文件将被清除, 不会对已修改和已删除的文件做任何处理

- 放弃 指定文件 添加
```
git clean <filename> –df
```
此时该新添加文件将被清除, 不会对已修改和已删除的文件做任何处理

- 放弃 指定文件夹 添加
```
git clean <directory> –df
```
此时该目录新添加文件将被清除, 不会对已修改和已删除的文件做任何处理

- 其他
> 忽略的文件：.gitignore 中忽略的文件；
> 未被跟踪的文件：没有被忽略，但是还没 git add 的文件

```
git clean  -f             # 删除：未被跟踪的文件
git clean –fd             # 删除：未被跟踪的文件和文件夹
git clean –xfd            # 删除：忽略的文件、未被跟踪的文件和文件夹
git clean [-xfd] -n-n     # 会先打印一些将要删除的文件，并不执行删除动作，主要是查看是否有自己需要的不想被删除
```

#### 更多``git reset``的用法

reset命令有3种方式：
>1：**git reset --mixed：** 此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息

>2：**git reset --soft：** 回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可

>3：**git reset --hard：** 彻底回退到某个版本，本地的源码也会变为上一个版本的内容


- 回退所有内容到上一个版本 
```
git reset HEAD^
``` 

- 回退a.py这个文件的版本到上一个版本
```
git reset HEAD^ a.py 
```

- 向前回退到第3个版本 
```
git reset –soft HEAD~3 
```

- 将本地的状态回退到和远程的一样 
```
git reset –hard origin/master 
```

- 回退到某个版本 
```
git reset 057d 
```

-回退到上一次提交的状态，按照某一次的commit完全反向的进行一次commit 
```
git revert HEAD 
```
### git比较本地仓库和远程仓库的差异
- 更新本地的远程分支
```
git fetch origin
```

- 本地与远程的差集 :（显示远程有而本地没有的commit信息）
```
git log master..origin/master
```

- 统计文件的改动
```
git diff <local branch> <remote>/<remote branch>
```
### git 对比两个分支差异
场景需求：假设有 2 个分支：master, dev，现在想查看这两个 branch 的区别，有以下几种方式：

- 查看 dev 有，而 master 中没有的：
```
git log dev ^master
```

- 查看 dev 中比 master 中多提交了哪些内容：
```
git log master..dev
```

- 不知道谁提交的多谁提交的少，单纯想知道有什么不一样：
```
git log dev...master
```

- 在上述情况下，再显示出每个提交是在哪个分支上：
```
git log --left-right dev...master
```
注意 commit 后面的箭头，根据我们在 –left-right dev…master 的顺序，左箭头 < 表示是 dev 的，右箭头 > 表示是 master的。

### large files

[Git Large File Storage](http://note.youdao.com/)

### issues
#### fatal: remote error: You can't push to git://github.com/test4581/test.git Use https://github.com/test4581/test.git

如果在git clone的时候用的是``git://github.com:xx/xxx.git ``的形式, 那么就会出现这个问题，因为这个protocol是不支持push的,用``git clone git@github.com:xx/xxx.git``就可以用git push了。

#### Merge branch 'master' of ...

[Git push 时如何避免出现 "Merge branch 'master' of ..."](https://www.cnblogs.com/Sinte-Beuve/p/9195018.html)
