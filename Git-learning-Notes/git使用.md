# Git的使用

[首先感谢廖雪峰大神的git学习教程](https://www.liaoxuefeng.com/wiki/896043488029600)

此文章作为一篇速查笔记，仅记录本人在git工具学习与使用过程中容易忘记，和需要查询的部分，如果需要系统性的学习，请点击上方廖雪峰大神的网站学习，或者前往[git官方网站下载官方文档学习](https://git-scm.com/book/zh/v2)

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

```git
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

```git
$ git add Test.txt

```

```git
$ git commit -m "b Test commit"
[master bfc296d] b Test commit
 1 file changed, 1 insertion(+), 1 deletion(-)

```

```git
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

```git
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

```git
$ git commit

```

```git
$ git checkout -- Test2.txt

```
