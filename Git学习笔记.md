参考：http://rogerdudler.github.io/git-guide/index.zh.html



# Git介绍

Git is a [free and open source](https://git-scm.com/about/free-and-open-source) distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

> Git是一个免费的开源分布式版本控制系统，旨在以速度和效率处理从小到大的项目。

# 工作流

你的本地仓库由 git 维护的三棵“树”组成。

第一个是你的 `工作目录`，它持有实际文件；第二个是 `暂存区（Index）`，它像个缓存区域，临时保存你的改动；最后是 `HEAD`，它指向你最后一次提交的结果。

![](%E5%9B%BE%E7%89%87/trees.png)

# 分支

分支是用来将特性开发绝缘开来的。在你创建仓库的时候，*master* 是“默认的”分支。在其他分支上进行开发，完成后再将它们合并到主分支上。

![](%E5%9B%BE%E7%89%87/branches.png)

创建一个叫做“feature_x”的分支，并切换过去：
`git checkout -b feature_x`
切换回主分支：
`git checkout master`
再把新建的分支删掉：
`git branch -d feature_x`
除非你将分支推送到远端仓库，不然该分支就是 *不为他人所见的*：
`git push origin <branch>`

> 可以这么理解。
> 例如，你的项目进行中遇到了一个问题，解决方案不确定，但是你不希望因此影响到当前的开发，那么你可以为此创建分支，然后在分支上测试你的方案，如果可行那么可以通过合并分支功能将你的更新应用到主干，反之你可以放弃它。

# 替换本地改动

假如你操作失误（当然，这最好永远不要发生），你可以使用如下命令替换掉本地改动：
`git checkout -- <filename>`
此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
`git fetch origin`
`git reset --hard origin/master`

# Git命令总结

## 获取 Git 仓库

1. 先创建文件夹

2. 然后右键执行`Git Bash Here`

3. 执行`git init`

   ​	在现有目录中初始化仓库（会在文件夹里创建一个.git的隐藏文件夹）

   或者

   执行`git clone [url]` directoryname：

   ​	从一个服务器克隆一个现有的 Git 仓库，directoryname（可空）自定义本地仓库名字

## 记录每次更新到仓库

**检测当前文件状态** ：

`git status`

**提出更改（把它们添加到暂存区**）：

`git add filename` (针对特定文件)、`git add *`(所有文件)、`git add *.txt`（支持通配符，所有 .txt 文件）

**忽略文件**：

`.gitignore` 文件

**提交更新**（你的改动已经提交到了 **HEAD**，但是还没到你的远端仓库）：

`git commit -m "代码提交信息"` （每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了， 然后再运行提交命令 `git commit`）

**跳过使用暂存区域更新的方式** ：

`git commit -a -m "代码提交信息"`。 `git commit` 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤。

**移除文件** ：`git rm filename` （从暂存区域移除，然后提交。）

**对文件重命名** ：

`git mv README.md README`(这个命令相当于`mv README.md README`、`git rm README.md`、`git add README` 这三条命令的集合)

## 推送改动到远程仓库

要更新你的本地仓库至最新改动，执行：`git pull`



如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：

`git remote add origin <server>` 

比如我们要让本地的一个仓库和 Github 上创建的一个仓库关联可以这样

`git remote add origin https://github.com/Snailclimb/test.git`

将将这些改动提交到远端仓库：

`git push origin master` (可以把 **master** 换成你想要推送的任何分支)



如此你就能够将你的改动推送到所添加的服务器上去了。

## 远程仓库的移除与重命名

将 test 重命名位 test1：`git remote rename test test1`

移除远程仓库 test1:`git remote rm test1`

## 查看提交历史

如果你想了解本地仓库的历史记录，最简单的命令就是使用: `git log`

你可以添加一些参数来修改他的输出，从而得到自己想要的结果。 

只看某一个人的提交记录：`git log --author=bob`

一个压缩后的每一条提交记录只占一行的输出：`git log --pretty=oneline`

或者你想通过 ASCII 艺术的树形结构来展示所有的分支, 每个分支都标示了他的名字和标签: 
`git log --graph --oneline --decorate --all`

看看哪些文件改变了: `git log --name-status`

这些只是你可以使用的参数中很小的一部分。更多的信息，参考：`git log --help`

## 撤销操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。

 此时，可以运行带有 `--amend` 选项的提交命令尝试重新提交：`git commit --amend`

取消暂存的文件：`git reset filename`

撤消对文件的修改：`git checkout -- filename`

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：

```
git fetch origingit reset --hard origin/master
```

## 分支

创建一个名字叫做 test 的分支：`git branch test`

切换当前分支到 test（当你切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。 Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样）：`git checkout test`

你也可以直接这样创建分支并切换过去(上面两条命令的合写)：`git checkout -b feature_x`

切换到主分支：`git checkout master`

合并分支(可能会有冲突)：` git merge test`

把新建的分支删掉：`git branch -d feature_x`

将分支推送到远端仓库（推送成功后其他人可见）：`git push origin `