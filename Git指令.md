

## Git常用的命令行指令：

---

Git是分布式版本控制系统

#### 在Windows上安装Git

直接下载Git Bash安装即可使用git，输入git检查是否安装了git

```
$ git
```

安装完成后，还需要最后一步设置，在命令行输入：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。



#### 创建版本库

初始化一个Git仓库

```
$ git init
```

添加文件到Git仓库，分两步：

第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；

第二步，使用命令git commit，完成。

```
$ git add readme.txt
$ git commit -m "wrote a readme file"
```

要随时掌握工作区的状态，使用git status命令

```
$ git status
```

如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

```
$ git diff readme.txt 
```

#### 版本回退

Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本

git log命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是append GPL，上一次是add distributed，最早的一次是wrote a readme file。
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

```
$ git log --pretty=oneline
3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
cb926e7ea50ad11b8f9e909c05226233bf755030 wrote a readme file
```

```
$ git reset --hard HEAD^   //回退到上一个版本
```

```
$ git reset --hard 3628164    //回退到某个指定commit id的版本
```

```
$ git reflog    //用来记录你的每一次命令
ea34578 HEAD@{0}: reset: moving to HEAD^
3628164 HEAD@{1}: commit: append GPL
ea34578 HEAD@{2}: commit: add distributed
cb926e7 HEAD@{3}: commit (initial): wrote a readme file
```



#### 撤销修改

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

```
$ git checkout -- readme.txt
```

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

```
$ git reset HEAD readme.txt    //用命令git reset HEAD file可以把暂存区的修改撤销掉，重新放回工作区
```

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。



#### 删除文件

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：

```
$ rm test.txt
```

确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：

```
$ git rm test.txt
$ git commit -m "remove test.txt"
```

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```
$ git checkout -- test.txt
```

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。



#### 添加远程库

要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

```
$ git remote add origin git@server-name:path/repo-name.git
```

关联后，使用命令git push -u origin master**第一次**推送master分支的所有内容；

```
$ git push -u origin master
```

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

```
$ git push origin master
```



#### 从远程库克隆

Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快

```
$ git clone git@github.com:michaelliao/gitskills.git
```



#### 创建与合并分支

```
$ git checkout -b dev  //创建并切换分支
```

```
$ git branch  //查看当前分支
$ git checkout master  //切换到master分支
```

```
$ git merge dev  //把dev分支的工作成果合并到master上
```

```
$ git branch -d dev  //删除分支dev
```

```
$ git log --graph --pretty=oneline --abbrev-commit  //可以看到分支的合并情况
```

```
$ git merge --no-ff -m "merge with no-ff" dev  //禁用Fast forward
```



#### Bug分支

工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
$ git stash
```

```
$ git stash list  //查看工作现场
```

恢复工作现场的方法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了：

```
$ git stash pop
```

你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：

```
$ git stash apply stash@{0}
```

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除

```
$ git branch -D feature-vulcan
```



#### 多人协作

多人协作的工作模式通常是这样：

首先，可以试图用git push origin branch-name推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。



查看远程库信息，使用git remote -v

```
$ git remote -v
```

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

```
$ git push origin branch-name
```

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

```
$ git checkout -b branch-name origin/branch-name
```

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

```
$ git branch --set-upstream branch-name origin/branch-name
```

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

```
$ git pull
```

