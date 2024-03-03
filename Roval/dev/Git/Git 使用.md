Created Date：2022-12-17 21:25:23  
Last Modified：2022-12-17 21:25:23

# Tags

#Git

# Content

## git push

`git push` 命用于从将本地的分支版本上传到远程并合并。

```
git push <远程主机名> <本地分支名>:<远程分支名>
```

如果本地分支名与远程分支名相同，则可以省略冒号：

```
git push <远程主机名> <本地分支名>
```

如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：

```
git push --force <远程主机名> <本地分支名>
```

`-u`

```
git push -u origin [target-branch]
```

本地分支与远程分支做管理，首次 push 到远程 registry 时指定默认 push 分支，后续 pull/push 操作可不用指定操作的分支。

`-d`

```
git push origin --delete feat/topic_ttt
```

删除远程主机上的分支

## git pull

`git pull = git fetch + git merge`

将远程分支拉下来并与本地分支合并。

```
git pull <远程主机名> <远程分支名>[:本地分支名]
```

eg. `git pull origin master:feat/topic_ttt `  
将远程 `origin` 的 `master` 分支拉下来与本地 `feat/topic_ttt` 分支合并。

## git commit

`git commit --amend` 修改最近一次提交的 **提交信息**

`gc --fixup [commitId]` 指定分支上 fix bug，然后 `grbi --autosquash [commitId]` 让 fix-bug-commit 分支合并到 `[commitId]` 指定那次 `commit` 里，从而隐去 fix-bug-commit

## git reset

回退到指定 CommitID 的版本

`--mixed` （默认）仅保留工作区代码。撤销上次 `git add` 和 `git commit` 操作（将添加到暂存区的内容取出！改动的文件颜色变回红色，工作区状态）

```bash
git reset --mixed HEAD^
等效
git reset HEAD^ // 默认
```

`--soft` 仅撤销上次 `commit`，保留 `git add` （暂存区和工作区代码保留！改动的文件颜色变回绿色，暂存区状态）

```bash
git reset --soft HEAD^
```

`--hard` 清除所有工作区、暂存区改动代码，撤销 `git add` 和 `git commit` 操作，是 `reset` 较为 **危险⚠️（丢失本次改动代码）** 的用法之一。（清除暂存区和工作区代码改动，改动的文件没了！）  

```bash
git reset --hard HEAD^
```

不像其他两种操作，难撤销，得通过 `git reflog` 找回。

### 总结

`--mixed` 回到 `git commit` 之前；  
`--soft` 回到 `git add` 之前；  
`--hard` 回到上次 `git commit` 之后；

## git diff

- `git diff HEAD <file_name>`
- `git diff <file_name>`
- `git diff --staged <file_name>` or  `git diff --cached <file_name>`,
- `git diff <branch_name1> <branch_name2> <file_name>`
- `git diff <commit_hash> <commit_hash> <file_name>`

## git rm

移除已添加到暂存区的代码

```shell
// 不删除物理文件,仅将该文件从缓存中删除
git rm --cached 文件路径 -r
// 删除物理文件,将该文件从缓存中删除
git rm --f 文件路径
```

## git rebase 变基

### 作用

- 作用一：减少分支上 `commit` 提交次数，比如一个 bug fix，中间修了很多次，提交了 3/4 次 `commit`，我想将它们合成一次 `commit`，我就可以 `grbi commitId`，其中 `commitId` 为 `previous commitId`。
- 作用二：从 `master` 上切分支后，在新的分支上一顿修改提交 `commit`，想要将所有 `commit` 合为一次的最快方法就是，`grbm` 即 `git rebase master`，这样 MR 时，master 上就干净很多了
- 作用三：code review & 便于回滚时 `commit` 选择

### 示意图

![[4iiIv.gif]]  

### 使用

`-i`：`--interactive` 弹出命令行以供用户选择

```bash
git rebase -i <commitId>
... 编辑配置文件
... 编辑提交信息
... git push origin <branch> -f // 强制提交，不要按提示pull来
```

编辑配置文件时，支持 `Commands` 有：

```bash
p, pick = use commit  
r, reword = use commit, but edit the commit message  
e, edit = use commit, but stop for amending  
s, squash = use commit, but meld into previous commit  
f, fixup = like "squash", but discard this commit's log message  
x, exec = run command (the rest of the line) using shell  
d, drop = remove commit
```

### 修改 Commit 信息

将某次提交前的 `pick` -> `edit`，然后按命令行提示完成剩余操作：

```bash
You can amend the commit now, with

  git commit --amend 

Once you are satisfied with your changes, run

  git rebase --continue

```

## git stash

保存当前所做修改到暂存区。

```bash
git stash
```

展示当前暂存区。

```bash
git stash list
```

应用某个暂存区修改到当前分支。

```bash
git stash pop stash@{x}
```

## git merge

### 概念

全分支合并，用于合并指定分支到当前分支。  
![[Wne9D.gif]]

```bash
git merge [--ff] <分支名> ## 默认 fast-forward
```

将分支的多次 `commit` 压缩到一起，然后合并到当前分支上。  
![[wzol8.gif]]

```bash
git merge --squash <分支名>
```

关闭 `fast-forward`，保留分支提交历史，且生成一次合并 `commit`。  
![[eWwAH.gif]]

```bash
git merge --no-ff <commitHash>
```

## git remote

本地仓库连接远程仓库

### 步骤

```bash
git remote // 查看远程
git remote rm origin // 先删除
git remote add origin git@github.com:HduSy/xxx.git // 再添加
```

## git cherry-pick

### 概念

部分合并，将指定的提交（`commit`）应用于其他分支。

```bash
git cherry-pick <commitHash>
```

## git filter-branch 核武器级选项

### 从每次提交中移除一个文件

```console
git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
```

## 问题

### fatal: refusing to merge unrelated histories

操作的命令后加上该 CLI 参数

```cmd
--allow-unrelated-histories
```

# Reference

[Git 命令简写](https://www.jianshu.com/p/660557b405dd)  
[阮一峰文章](https://www.ruanyifeng.com/blog/2015/08/git-use-process.html)  
[阮一峰 cherry-pick](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html)  
[Git - 重写历史](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E5%86%99%E5%8E%86%E5%8F%B2)  
[git filter-branch (Administration) - Git 中文开发手册 - 开发者手册 - 腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/section/1138641)
