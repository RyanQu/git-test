#Git学习笔记

##Git仓库
Git文件夹下的三个项目：`festock`, `WIEmaterial`, `git-test`均为不同的git仓库
可以理解为每一个单独的Project都是一个仓库

##创建和提交：Add & Commit
在文件夹下创建一个`README.md`文件，之后执行Shell命令：

```
➜  git-test git:(test) ✗ git add README.md
➜  git-test git:(test) ✗ git commit -m "wrote a readme file"
[test 692e65b] wrote a readme file
 1 file changed, 6 insertions(+)
 create mode 100644 README.md
```

`git add`旨在给Git发送指令将此文件添加至Git仓库
`git commit`旨在给Git发送指令提交所有更改（可以一次`commit`多个`add`）

##修改文件：Status & Diff
修改`README.md`，之后执行Shell命令：

```
➜  git-test git:(test) git status
On branch test
Your branch is ahead of 'origin/test' by 1 commit.
  (use "git push" to publish your local commits)
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
➜  git-test git:(test) ✗ git diff README.md
```

`git status`可以检查当前的仓库状态.

`git diff`可以检查某文件与旧版本的异同。执行之后需要重复`add`和`commit`的操作，进行提交。

感觉执行`git diff`命令之后，在shell中打开了一个文本文件类型，使用方式像Vim，按q退出。

如果不进行`add`直接`commit`会出现如下的错误，显示`No changes add to commit`。看来必须是`add`然后`commit`的顺序。

```
➜  git-test git:(test) ✗ git commit -m "Modificaiton"
On branch test
Your branch is ahead of 'origin/test' by 1 commit.
  (use "git push" to publish your local commits)
Changes not staged for commit:
	modified:   README.md

no changes added to commit
➜  git-test git:(test) ✗ git add README.md
➜  git-test git:(test) ✗ git commit -m "Modificaiton"
[test 6937b72] Modificaiton
 1 file changed, 4 insertions(+), 1 deletion(-)
```

##版本控制：log, reset & reflog
每个`commit`可以理解为一个快照，可以回滚至任一`commit`的版本。

```
➜  git-test git:(test) ✗ git log
```

`git log`可以输出版本控制系统的历史记录，如下：

```
commit 6937b72c8563173d832959acbec8f5081d84f341
Author: RyanQu <ryanqu94@sina.com>
Date:   Wed Jan 4 09:59:40 2017 -0500

    Modificaiton

commit 692e65b01297514a86ee1178621c00591bf7800c
Author: RyanQu <ryanqu94@sina.com>
Date:   Wed Jan 4 09:43:59 2017 -0500

    wrote a readme file

commit 1bbcd0488035eb0b3021585b576483b3044b56ad
Merge: 5f49a8f 8ce79b5
Author: RyanQu <ryanqu94@sina.com>
Date:   Mon Jan 2 20:36:22 2017 -0500

    Merge branch 'test-1' into test

commit 8ce79b5cb8b951c27f66a38de7c1a742306e5db8
Author: RyanQu <ryanqu94@sina.com>
Date:   Mon Jan 2 20:17:42 2017 -0500

:
```

在输出`log`的界面，依然是输入q退出，还是不太懂这个输出的是什么格式。和shell不太一样的。

可以通过命令参数`--pretty=oneline`来调整log的输出样式，如下：

```
➜  git-test git:(test) git log --pretty=oneline
```

```
53c7fdd77d23695903aeeb96eb9123c311d7f4c9 Update: 2nd modification to README.md
6937b72c8563173d832959acbec8f5081d84f341 Modificaiton
692e65b01297514a86ee1178621c00591bf7800c wrote a readme file
1bbcd0488035eb0b3021585b576483b3044b56ad Merge branch 'test-1' into test
8ce79b5cb8b951c27f66a38de7c1a742306e5db8 Add: a meaningless image
5f49a8f051d371dc0b6a01997216d322052ccb04 Add: mooc.xls
98655ae02725253d4fade8d38332850eba79a81e Add: data_ana.xls
(END)
```

感觉`--pretty`这个参数望文生义的，确实单行显示的话格式更整齐的

可以通过执行`git reset`命令来进行版本的回滚，如下：

```
➜  git-test git:(test) git reset --hard HEAD^                     
HEAD is now at 6937b72 Modificaiton

```

需要注意这个地方回滚版本的控制，在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，往上n个版本写成`HEAD~n`。

当希望撤销回滚的时候，需要明确版本号是什么，就是那个非常长的一串乱码。因为在目前的`log`中，被撤销的那个版本已经不见了，所以不能通过`Head`来进行回滚。

Git提供了`git reflog`命令来记录每一次命令，如下：

```
➜  git-test git:(test) git reflog
```

```
6937b72 HEAD@{0}: reset: moving to HEAD^
53c7fdd HEAD@{1}: commit: Update: 2nd modification to README.md
6937b72 HEAD@{2}: commit: Modificaiton
692e65b HEAD@{3}: commit: wrote a readme file
1bbcd04 HEAD@{4}: checkout: moving from test-1 to test
8ce79b5 HEAD@{5}: checkout: moving from test to test-1
1bbcd04 HEAD@{6}: merge test-1: Merge made by the 'recursive' strategy.
5f49a8f HEAD@{7}: checkout: moving from test-1 to test
8ce79b5 HEAD@{8}: checkout: moving from test to test-1
5f49a8f HEAD@{9}: checkout: moving from test-1 to test
8ce79b5 HEAD@{10}: commit: Add: a meaningless image
5f49a8f HEAD@{11}: checkout: moving from test to test-1
5f49a8f HEAD@{12}: checkout: moving from test-1 to test
5f49a8f HEAD@{13}: checkout: moving from test to test-1
5f49a8f HEAD@{14}: commit: Add: mooc.xls
98655ae HEAD@{15}: commit (initial): Add: data_ana.xls
(END)
```

这时查找回退的版本对应`commit`的版本号是53c7fdd，再次执行`reset`命令，如下：

```
➜  git-test git:(test) git reset --hard 53c7fdd
HEAD is now at 53c7fdd Update: 2nd modification to README.md
```

检查log文件，发现已经撤销回滚成功，如下：

```
commit 53c7fdd77d23695903aeeb96eb9123c311d7f4c9
Author: RyanQu <ryanqu94@sina.com>
Date:   Fri Jan 6 15:16:59 2017 -0500

    Update: 2nd modification to README.md
```

`Head`的本质是指针，指向当前版本。当进行回滚的时候，只是指针的位置发生了改变。

个人理解是所有的版本信息都已经被记录了，只是能否通过正常的`Head`指针读取的问题。

##暂存区、工作区、版本库
工作区即是该项目代码所在的目录，在这个过程中就`git-test`这个文件夹。

在此文件夹下存在一个`.git`目录，默认隐藏，`ls`命令看不到，需要用`ls -ah`来查看，如下：

```
➜  git-test git:(test) ls
README.md    data_ana.xls logo.png.png mooc.xls
➜  git-test git:(test) ls -ah
.            .DS_Store    README.md    logo.png.png
..           .git         data_ana.xls mooc.xls
```

`.git`目录作为Git的版本库，不属于工作区。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

`git add`命令是把文件添加进去，实际上就是把文件修改添加到暂存区；

`git commit`命令提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以`git commit`就是往`master`分支上提交更改。

可以理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

同时，Git是跟踪修改的，每次修改，如果不`add`到暂存区，那就不会加入到`commit`中。

##撤销修改 
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`，如下：

```
➜  git-test git:(test) ✗ git status         
On branch test
Your branch is ahead of 'origin/test' by 1 commit.
  (use "git push" to publish your local commits)
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
➜  git-test git:(test) ✗ git checkout -- README.md
```

修改已被撤销。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。

`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了`git checkout`，是“切换到另一个分支”的命令.

##删除 rm
在Git中，删除也是一个修改操作，先添加一个新文件`text.md`到Git并且提交：

```
➜  git-test git:(test) git status
On branch test
Your branch is ahead of 'origin/test' by 1 commit.
  (use "git push" to publish your local commits)
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	text.md

nothing added to commit but untracked files present (use "git add" to track)
➜  git-test git:(test) ✗ git add text.md
➜  git-test git:(test) ✗ git commit -m "add text.md"
[test ba33bcd] add text.md
 1 file changed, 12 insertions(+)
 create mode 100644 text.md
```

直接使用`rm`命令可以在系统中删除该文件，删除之后导致工作区和版本库不一样，可以通过`git staus`命令查看，如下：

```
➜  git-test git:(test) ✗ git status
On branch test
Your branch is ahead of 'origin/test' by 2 commits.
  (use "git push" to publish your local commits)
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    text.md

no changes added to commit (use "git add" and/or "git commit -a")
```

现在有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`，如下：

```
➜  git-test git:(test) ✗ git rm text.md
rm 'text.md'
➜  git-test git:(test) ✗ git commit -m "remove text.md"
[test aed4f4b] remove text.md
 1 file changed, 12 deletions(-)
 delete mode 100644 text.md
```

现在，文件就从版本库中被删除了。

另一种情况是删错了，因为版本库里还有，所以可以把误删的文件恢复到最新版本，如下：

```
➜  git-test git:(test) git checkout -- test.txt
```
即可一键还原文件。

##关联Github push
根据GitHub的提示，在本地的`xxx`仓库下运行命令：

```
$ git remote add origin git@github.com:Gitusername/xxx.git
```

此处命令只是做一个备份，之前在客户端上做好了关联操作。

下面来进行`push`操作。编辑`README.md`，检查`git status`，并`commit`，如下：

```
➜  git-test git:(test) git status
On branch test
Your branch is up-to-date with 'origin/test'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
➜  git-test git:(test) ✗ git add README.md
➜  git-test git:(test) ✗ git commit -m "Update to push"
[test 76ab6aa] Update to push
 1 file changed, 3 insertions(+), 1 deletion(-)
```

之后执行`push`操作，如下：

```
➜  git-test git:(test) git push -u origin test  
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 383 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local objects.
To https://github.com/RyanQu/git-test.git
   aed4f4b..76ab6aa  test -> test
Branch test set up to track remote branch test from origin.
```

第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

```
git push origin master
```

##克隆 clone

找了一个原来的空项目，改个名字，`jd_crawler`，用作克隆的练习，也做第一个整理的项目，关于京东爬虫。

在GitHub网站上弄好项目和`README.md`，开始准备克隆。

```
➜  Git git clone git@github.com:RyanQu/jd_crawler.git
Cloning into 'jd_crawler'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
➜  Git ls -ah
.           .DS_Store   festock     jd_crawler
..          WIEmaterial git-test
```

克隆完毕，进入`jd_crawler`这个文件夹，发现`README.md`已经克隆下来了。

```
➜  Git cd jd_crawler
➜  jd_crawler git:(master) ls -ah
.         ..        .git      README.md
```

除了刚才用到的ssh登陆的方式，还可以通过`https`请求。

GitHub给出的地址不止一个，还可以用`https://github.com/RyanQu/jd_crawler.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令。

##分支 branch & checkout & merge

分支的概念其实还是好理解的。一个项目有一条主的Timeline，所有关于这个项目的提交都在这个上面，完成一期、二期、n期工程之后，项目结束。

在这个过程中，需要不同的工种来进行配合，诸如前端、后端之类的。每完成一部分工作就提交至整体项目是很危险的，一来不稳定甚至是不可用的，二来当需要进行回滚的时候很大可能也会回滚掉别人的代码。

分支的作用体现在这里，不同的工种，或者系统的不同功能，作为一个新的分支。这个分支与主分支、其他分支都不影响，自己闭门造车，在完成一部分相对完整的工作之后再提交至主分支。

创建新的分支`dev`，并切换至`dev`，如下：

```
➜  jd_crawler git:(master) git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```
➜  jd_crawler git:(master) git branch dev
➜  jd_crawler git:(master) git checkout dev
Switched to branch 'dev'
```

然后，用`git branch`命令查看当前分支：

```
➜  jd_crawler git:(dev) git branch
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个*号。

在`dev`分支下修改并提交文件，如下：

```
➜  jd_crawler git:(dev) git add README.md
➜  jd_crawler git:(dev) ✗ git commit -m "Add Author info"
[dev b1e9f55] Add Author info
 1 file changed, 3 insertions(+), 2 deletions(-)
```

`dev`分支下的工作结束，切换回`master`，并合并`dev`到当前分支，如下：

```
➜  jd_crawler git:(dev) git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
➜  jd_crawler git:(master) git merge dev
Updating 563025c..b1e9f55
Fast-forward
 README.md | 5 +++--
 1 file changed, 3 insertions(+), 2 deletions(-)
```

`git merge`命令用于合并指定分支到当前分支。这里使用的是`Fast-forward`合并模式。

合并完成后，删除`dev`分支，并查看`branch`，如下：

```
➜  jd_crawler git:(master) git branch -d dev
Deleted branch dev (was b1e9f55).
➜  jd_crawler git:(master) git branch
* master
```

执行`push`，同步至GitHub，如下：

```
➜  jd_crawler git:(master) git push
Counting objects: 3, done.
Writing objects: 100% (3/3), 285 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:RyanQu/jd_crawler.git
   563025c..b1e9f55  master -> master
```

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用`git log --graph`命令可以看到分支合并图。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

##Bug处理 Stash
Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。

用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

修复完成后，工作区是干净的，刚才的工作现场用`git stash list`命令查看。

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了。

##多人协作

查看远程库信息，使用`git remote -v`；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin branch-name`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！
5. 如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

#标签管理 Tag
发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

命令`git tag <name>`用于新建一个标签，默认为HEAD，也可以指定一个`commit id`；

`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

`git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；

命令`git tag`可以查看所有标签。

命令`git push origin <tagname>`可以推送一个本地标签；

命令`git push origin --tags`可以推送全部未推送过的本地标签；

命令`git tag -d <tagname>`可以删除一个本地标签；

命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

##忽略文件
忽略某些文件时，需要编写`.gitignore`；

`.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！