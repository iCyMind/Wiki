# Git

假设有多个提交:

```bash
$ git log --pretty=oneline
a931ac7c808e2471b22b5bd20f0cad046b1c5d0d c
b76d157d507e819d7511132bdb5a80dd421d854f b
df239176e1a2ffac927d8b496ea00d5488481db5 a
```

a, b, c 三个提交依次向 demo.txt 添加了a, b, c 三行文字

## HEAD or head?

head 是否区分大小写取决于你的文件系统, 如果文件系统 case-sensitive 的如 linux, 必须要写 HEAD;
如果是 Windows, 那么无所谓;
macOS 可以是两种情况都有.

所以为了一致性, 写 HEAD 最为稳妥.

## 合并多个提交

要将 c 合并到 b, 使得两个提交变成一个. 使用 `git rebase -i df2391` 或者 `git rebase -i HEAD~2`.
然后在弹出来的vim中将 c 提交前的 pick 改为 squash 即可.

注意 rebase 的参数填的的是 rebase 的基础, 在这个场景中, 提交 a 就是基础, 因此参数写的是 HEAD~2.

a 是首次提交, 如果想把 a/b/c 合并为一个提交, 那么 rebase 的参数又是什么呢? 因为 HEAD~2 已经是到头了, 因此只能用一个特殊的参数 --root 来表示 a 提交前的状态:

`git rebase -i --root`

## 修改提交的信息

要修改上次提交的信息, 执行 `git commit --amend` 即可. 如果需要修改上上次的提交信息, 那只能依靠 rebase 了.
在执行 rebase 命令后弹出的 vim 窗口中将对应的提交前的 pick 改为 reword 即可.

## 回滚到历史版本

git reset 的 mixed, hard, soft 选项都会修改 HEAD 的指向, 区别在于对 Stage/Index 区和 Working directory 的影响不同.

假设当前 HEAD 指向 c

```bash
# Index/WD 不变. 再执行一遍 git commit 即可到达提交 c
git reset --soft  HEAD~1

# 改变 Index, WD 不变. 需要执行 git add demo.md 将文件添加到 Index, 再 git commit 才能到达提交 c
git reset --mixed HEAD~1

#  改变 Index 以及 WD, 需要再往 demo.md 添加 "c", 然后 git add demo.md, 最后 commit 才能变成 c
git reset --hard  HEAD~1

```

## 切换到一个[本地没有的远程才有的]分支

`git check --track origin/test`

## 取消所有更改

If you want to revert changes made to your working copy, do this: `git checkout .`

If you want to revert changes made to the index (i.e., that you have added), do this. Warning this will reset all of your unpushed commits to master!: `git reset`

If you want to revert a change that you have committed, do this: `git revert <commit 1> <commit 2>`

If you want to remove untracked files (e.g., new files, generated files): `git clean -f`

Or untracked directories (e.g., new or automatically generated directories): `git clean -fd`
