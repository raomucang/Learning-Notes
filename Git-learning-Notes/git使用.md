# Git的使用

[首先感谢廖雪峰大神的git学习教程](https://www.liaoxuefeng.com/wiki/896043488029600)

此文章作为一篇速查笔记，仅记录本人在git工具学习与使用过程中容易忘记，和需要查询的部分，如果需要系统性的学习，请点击上方廖雪峰大神的网站学习，或者前往[git官方网站下载官方文档学习](https://git-scm.com/book/zh/v2)

## Git命令速查

`git init`  建立Git仓库

`git add fileName`  将文件加入到暂存区

`git commit -m "本次提交的说明"`  将文件提交到仓库

`git status`  显示工作区状态

`git diff`  显示那些文件被修改过

`git log` 显示从最近到最远的提交日志

`git log --pretty=oneline`  显示从最近到最远的提交日志简单版

`git reset --hard HEAD^`  回退到上一个版本

`git reset --hard HEAD^^` 回退到上上个版本

`git reset --hard HEAD~100` 回退到往上100个版本

`git reset --hard 版本号` 回退到某个版本

`git reflog`  查看命令历史

`git checkout -- fileName`  撤销文件在工作区的修改

`git rm fileName` 删除文件

`git checkout -- fileName`  恢复文件资源管理器中删除的文件

`git remote add 远程仓库名 仓库地址.git`  关联远程仓库

`git push 远程仓库名/分支名 分支名` 将本地选中的分支推送到远程仓库选中的分支中

`git push -u 远程仓库名/分支名 分支名`  加上`-u`参数，可以将本地仓库分支和远程仓库分支关联起来，在以后的推送或者拉取时就可以简化命令。

`git push 远程仓库名 分支名`  简化后的推送命令

`git clone 远程库的地址`  从远程克隆一个库到本地

`git checkout 分支名` 如果有分支，则切换到目标分支，没有则创建一个

`git checkout -b 分支名`  `-b`参数代表创建并切换

`git branch`  查看当前分支

`git merge 分支名`  将选中分支合并到当前分支

`git merge --no-ff -m "merge with no-ff" dev` 禁用`Fast forward`模式

`git branch -d 分支名` 删除选中分支

`git branch -D 分支名`  强行删除还未合并的分支

`git log --graph`  查看分支合并情况

`git log --graph --pretty=oneline --abbrev-commit`  查看分支合并情况

`git stash` 存储工作现场

`git stash list`  查看存储的工作现场

`git stash apply` 恢复但不删除`stash`

`git stash pop` 恢复并删除`stash`

`git stash apply stash@{0}` 恢复指定的`stash`

`git cherry-pick 提交编号` 复制一个特定的提交到当前分支

`git remote`  查看远程库的信息

`git remote -v` 查看远程库的详细信息

`git pull`  从一个仓库或者本地的分支拉取并且整合代码

`git rebase`  整理提交历史

`git tag 标签名` 打一个新标签

`git tag` 查看所有标签

`git tag 标签名 提交序号` 给一个特定的提交打一个标签

`git tag -d 标签名` 删除标签

`git push 仓库名 标签名`  推送某个标签到远程

`git push origin --tags`  一次性推送全部尚未推送到远程的本地标签

`git push origin :refs/tags/标签名` 删除远程标签

## 创建版本库

新建一个文件夹，在文件夹下输入 `git init` 命令会将此文件夹变为Git可以管理的仓库,并在此文件夹下会生成一个名为 `.git` 的新目录（默认是隐藏的）,此目录是Git用来跟踪和管理版本库

```git
$ git init
Initialized empty Git repository in D:/gitSpace/Test/.git/
```

## 一个基本的流程工作

### 工作区（Working Directory）

首先我们要明白工作区就是我们正在进行工作的区域，对于文件的修改是在工作区进行的

### 暂存区（Stage）

每当我们完成一部分工作（例如：新建文件，删除文件，修改文件等），使工作区发生改变，我们都可以使用 `git add` 命令将工作区的当前状况（改变后的状况）添加到暂存区。在这里我们在工作区中新建了 `Test.txt` 和 `Test2.txt` 两个文件，然后将 `Test.txt` 文件加入到暂存区


```git
$ git add Test.txt
```


我们可以使用 `git status` 命令查看当前状态，这里Git告诉我们 `Test.txt` 文件被修改了，而 `Test2.txt` 文件 没有使用 `git add` 命令添加过，所以它的状态是 `Untracked` 。

```git
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   Test.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        Test2.txt
```

现在，我们再次使用 `git add` 命令将 `Test.txt` 也添加到暂存区，然后再使用 `git status` 命令查看一下，我们可以看到两个文件现在都在暂存区了

```
$ git add Test2.txt
```

```git
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   Test.txt
        new file:   Test2.txt
```

### 提交更改

一般情况，每当完成一个阶段的工作，一个bug的修复，一个功能的添加，一个版本的更迭等，我们就需要将暂存区中的文件提交更改。提交更改就是把暂存区的所有内容提交到当前分支，可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。在这里，我们使用 `git commit` 命令

```git
$ git commit -m "a Test commit"
[master (root-commit) d52d241] a Test commit
 2 files changed, 2 insertions(+)
 create mode 100644 Test.txt
 create mode 100644 Test2.txt
```

再次使用 `git status` 命令看看，可以看到已经将暂存区的文件全部提交了

```git
$ git status
On branch master
nothing to commit, working tree clean
```

## 版本回退

每当我们使用 `git commit` 命令，都是在Git中“保存了一个快照”，可以理解为一次游戏存档，如果我们将文件该乱了，我们就可以从 `commit` 中恢复，然后继续工作。
为了接下来演示方便我们再进行两次文件修改和提交次 `commit` ，分别加入说明 `b Test commit` 和 `c Test commit`

```
$ git add Test.txt
```

```git
$ git commit -m "b Test commit"
[master bfc296d] b Test commit
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```
$ git add Test.txt
```

```git
$ git commit -m "c Test commit"
[master fe34aea] c Test commit
 1 file changed, 1 insertion(+), 1 deletion(-)
```

我们可以使用 `git log` 命令查看 `commit` 的历史记录，`git log` 命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`c Test commit`，上一次是`b Test commit`，最早的一次是`a Test commit`。

```git
$ git log
commit fe34aea35ace7505080b3c233b5389b3e1d7bb0a (HEAD -> master)
Author: raomucang <2267168887@qq.com>
Date:   Sat Aug 24 16:17:31 2019 +0800

    c Test commit

commit bfc296d26c586420f7a48ea1805e241408e4cbce
Author: raomucang <2267168887@qq.com>
Date:   Sat Aug 24 16:17:11 2019 +0800

    b Test commit

commit d52d2415afe6304a28d1065ff4d29cf6fc30d414
Author: raomucang <2267168887@qq.com>
Date:   Sat Aug 24 15:58:43 2019 +0800

    a Test commit
```

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上 `--pretty=oneline` 参数，其中你所看到 `fe34aea3...` 这样一大串的是版本号

```git
$ git log --pretty=oneline
fe34aea35ace7505080b3c233b5389b3e1d7bb0a (HEAD -> master) c Test commit
bfc296d26c586420f7a48ea1805e241408e4cbce b Test commit
d52d2415afe6304a28d1065ff4d29cf6fc30d414 a Test commit
```

现在我们要使用 `git reset` 命令回退到上一个版本，也就是 `b Test commit` 版本，这里我们使用的完整命令是 `git reset --hard HEAD^`，如果要回退到上上个版本则是`git reset --hard HEAD^^`，如果回退版本次数多，还可以写为 `git reset --hard HEAD~100` 这样的形式

```git
$ git reset --hard HEAD^
HEAD is now at bfc296d b Test commit
```

再次使用 `git log` 查看版本库的状态，最新的版本 `c Test commit` 已经找不到了，但是如果我们想回到 `c Test commit` ,我们可以使用 `git reset --hard 版本号前几位` 命令，如果不知道版本号可以使用 `git reflog` 命令来查询你的每一次命令

```git
$ git log
commit bfc296d26c586420f7a48ea1805e241408e4cbce (HEAD -> master)
Author: raomucang <2267168887@qq.com>
Date:   Sat Aug 24 16:17:11 2019 +0800

    b Test commit

commit d52d2415afe6304a28d1065ff4d29cf6fc30d414
Author: raomucang <2267168887@qq.com>
Date:   Sat Aug 24 15:58:43 2019 +0800

    a Test commit
```

```git
$ git reset --hard fe34a
HEAD is now at fe34aea c Test commit
```

```git
$ git reflog
fe34aea (HEAD -> master) HEAD@{0}: reset: moving to fe34a
bfc296d HEAD@{1}: reset: moving to HEAD^
fe34aea (HEAD -> master) HEAD@{2}: commit: c Test commit
bfc296d HEAD@{3}: commit: b Test commit
d52d241 HEAD@{4}: commit (initial): a Test commit
```

## 撤销修改

`git checkout --file`命令可以丢弃工作区的修改（注意 `--`），就是让工作区文件和暂存区文件保持一致，就是让这个文件回到最近一次git commit或git add时的状态。

```
$ git checkout --Test.txt
```

## 删除文件

在文件资源管理器中删除文件后，在Git中使用 `git status` 命令会立即告诉你那些文件被删除了，现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令 `git rm` 删掉，并且`git commit`，另一种情况是删错了，因为版本库里还有呢，所以可以很轻松使用 `$ git checkout -- file` 命令把误删的文件恢复到最新版本

```git
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    Test2.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

```git
$ git rm Test.txt
rm 'Test.txt'
```

`$ git commit`

`$ git checkout -- Test2.txt`

## 远程仓库的使用

### 关联远程仓库

如果你想将本地仓库与远程仓库关联起来可以使用 `git remote add 远程仓库名（如果使用的是github这里一般是origin） 远程仓库地址.git` 命令，然后我们可以使用 `git remote -v` 命令查看已添加的库

```
$ git remote add origin https://github.com/raomucang/Java-Learning-Notes.git
```

```git
$ git remote -v
origin  https://github.com/raomucang/Java-Learning-Notes.git (fetch)
origin  https://github.com/raomucang/Java-Learning-Notes.git (push)
```

### 将本地库的所有内容推送到远程库上

使用 `git push` 命令将本地库的内容推送到远程库上，这里使用的 `git push -u origin master` 命令中 `origin` 是指远程库的名字，而 `master` 是指远程库分支名，`master` 一般是主分支名，实际上是把当前分支推送到远程分支 `master`

由于远程库是空的，我们第一次推送 `master` 分支时，加上了`-u`参数，Git不但会把本地的 `master` 分支内容推送的远程新的 `master` 分支，还会把本地的 `master` 分支和远程的 `master` 分支关联起来，在以后的推送或者拉取时就可以简化命令。

```git
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

从现在起，只要本地作了提交，就可以通过命令

```
$ git push origin master
```

### 当推送时出现冲突

在多人合作的项目中，有时你推送时会出现冲突，这时我们就需要先使用 `git pull` 命令将最新的提交抓下来，然后在本地合并，解决冲突，再推送

```git
$ git push origin dev
To github.com:michaelliao/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

```git
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：

```git
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再次 `git pull`

```git
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这回 `git pull` 成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样，需要打开文件解决。解决后，提交 `commit`，再 `git push`：

```git
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

### 从远程库克隆

使用命令 `git clone 远程仓库地址` 克隆一个本地库

```git
$ git clone https://github.com/raomucang/Java-Learning-Notes.git
Cloning into 'Java-Learning-Notes'...
remote: Enumerating objects: 62, done.
remote: Counting objects: 100% (62/62), done.
remote: Compressing objects: 100% (38/38), done.
remote: Total 62 (delta 25), reused 49 (delta 15), pack-reused 0
Unpacking objects: 100% (62/62), done.
```

## 分支管理

### 创建分支

我们创建一个名为 `dev` 的分支，然后切换到 `dev` 分支：使用命令`$ git checkout -b dev`

```git
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout` 命令加上 `-b` 参数表示创建并切换，相当于以下两条命令

```git
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

### 查看当前分支

`git branch` 命令会列出所有分支，当前分支前面会标一个 `*` 号，现在我们的提交都是在 `dev` 分支上

```git
$ git branch
* dev
  master
```

### 切换分支

`git checkout 分支名` 命令还可以用来切换分支，这里我们使用 `git checkout master` 可以切换到 `master` 分支

```git
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

### 合并分支

我们可以使用 `git merge 想要合并的分支名` 命令可以将想要合并的分支合并到当前分支上

```git
$ git merge dev
Already up to date.
```

通常，合并分支时，如果可能， `Git` 会用``Fast forward``模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用 `Fast forward` 模式， `Git` 就会在 `merge` 时生成一个新的 `commit` ，这样，从分支历史上就可以看出分支信息。

```git
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

### 删除分支

我们可以使用 `git branch -d 分支名` 命令删除分支

```git
$ git branch -d dev
Deleted branch dev (was f1988e8).
```

删除后，查看 `branch` ，就只剩下 `master` 分支了：

```git
$ git branch
* master
```

## 冲突解决

当我们分别在两个分支上有了不同的 `commit` ，这时我们想要合并分支，便会报错

```git
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这时我们使用 `git status` 命令可以查看冲突位置

```git
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

这时我们便需要手动打开文件修改冲突位置，Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容

```git
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

修改之后我们再用 `git add 文件名` 命令暂存，然后使用 `git commit -m "conflict fixed"` 命令提交

```git
$ git add readme.txt
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

## 隐藏当前工作现场

我们可以使用 `git stash` 命令，将当前工作现场“存储起来”，等以后恢复现场后继续工作

```git
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

需要恢复工作现场时，使用 `git stash list` 命令查看

```git
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

恢复工作现场，有两个办法：
一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
另一种方式是用git stash pop，恢复的同时把stash内容也删了：

```git
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

你可以多次`stash`，恢复的时候，先用 `git stash list` 查看，然后恢复指定的 `stash` ，用命令：

```
$ git stash apply stash@{0}
```

## 复制一个特定的提交到当前分支

```git
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

## 删除未合并分支

未合并分支删除时需要使用大写 `-D` 参数

```git
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```

## 创建远程分支到本地

创建远程 `origin` 的 `dev` 分支到本地，于是他用这个命令创建本地 `dev` 分支

```
$ git checkout -b dev origin/dev
```

## 我是小尾巴

 你已经学会了Git的基基基基本操作了，已经是一个成熟的Git初学者了，要学会自己发现问题，寻找问题，本笔记会继续更新其他关于git的其他用法，但是如果你学的速度超过了本笔记的更新速度。。。你可以去看[廖雪峰大神的git学习教程](https://www.liaoxuefeng.com/wiki/896043488029600)或者前往[git官方网站下载官方文档学习](https://git-scm.com/book/zh/v2)
