[TOC]
# Github使用教程

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
[link](http://www.tech126.com/git-reset/)

> git stash 可用来暂存当前正在进行的工作， 比如想pull 最新代码， 又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作。


基础命令：
```
git stash
do some work
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
### 从远程的分支获取最新的版本到本地有这样2个命令

- git fetch：相当于是从远程获取最新版本到本地，不会自动merge
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

- git pull：相当于是从远程获取最新版本并merge到本地
``` 
git pull origin master
```
上述命令其实相当于git fetch 和 git merge在实际使用中，git fetch更安全一些 因为在merge前，我们可以查看更新情况，然后再决定是否合并.

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

### 版本恢复的各种场景
不小心``git add``了不希望add的文件，可以使用``git reset``撤回
>reset命令有3种方式：

>1：**git reset –mixed：** 此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息

>2：**git reset –soft：** 回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可

>3：**git reset –hard：** 彻底回退到某个版本，本地的源码也会变为上一个版本的内容

```
#回退所有内容到上一个版本 
git reset HEAD^ 
#回退a.py这个文件的版本到上一个版本 
git reset HEAD^ a.py 
#向前回退到第3个版本 
git reset –soft HEAD~3 
#将本地的状态回退到和远程的一样 
git reset –hard origin/master 
#回退到某个版本 
git reset 057d 
#回退到上一次提交的状态，按照某一次的commit完全反向的进行一次commit 
git revert HEAD 
```

如果我们某次修改了某些内容，并且已经commit到本地仓库，而且已经push到远程仓库了
这种情况下，我们想把本地和远程仓库都回退到某个版本，该怎么做呢？
前面讲到的git reset只是在本地仓库中回退版本，而远程仓库的版本不会变化
这样，即使本地reset了，但如果再git pull，那么，远程仓库的内容又会和本地之前版本的内容进行merge
这并不是我们想要的东西，这时可以有2种办法来解决这个问题：
1：直接在远程server的仓库目录下，执行git reset –soft 10efa来回退。注意：在远程不能使用mixed或hard参数
2：在本地直接把远程的master分支给删除，然后再把reset后的分支内容给push上去，如下：
```
#新建old_master分支做备份  
git branch old_master  
#push到远程  
git push origin old_master:old_master  
#本地仓库回退到某个版本  
git reset –hard bae168  
#删除远程的master分支  
git push origin :master  
#重新创建master分支  
git push origin master  
```
在删除远程master分支时，可能会有问题，见下：
```
$ git push origin :master  
error: By default, deleting the current branch is denied, because the next  
error: 'git clone' won't result in any file checked out, causing confusion.  
error:  
error: You can set 'receive.denyDeleteCurrent' configuration variable to  
error: 'warn' or 'ignore' in the remote repository to allow deleting the  
error: current branch, with or without a warning message.  
error:  
error: To squelch this message, you can set it to 'refuse'.  
error: refusing to delete the current branch: refs/heads/master  
To git@xx.sohu.com:gitosis_test  
 ! [remote rejected] master (deletion of the current branch prohibited)  
error: failed to push some refs to 'git@xx.sohu.com:gitosis_test'
```
这时需要在远程仓库目录下，设置git的receive.denyDeleteCurrent参数
```
git receive.denyDeleteCurrent warn
```
然后，就可以删除远程的master分支了
虽然说有以上2种方法可以回退远程分支的版本，但这2种方式，都挺危险的，需要谨慎操作


### large files

[Git Large File Storage](http://note.youdao.com/)

### issue
#### fatal: remote error: You can't push to git://github.com/test4581/test.git Use https://github.com/test4581/test.git

如果在git clone的时候用的是``git://github.com:xx/xxx.git ``的形式, 那么就会出现这个问题，因为这个protocol是不支持push的,用``git clone git@github.com:xx/xxx.git``就可以用git push了。
