很好用的commit管理命令

```sh
git rebase -i HEAD~2
#选择需要的范围
```

使用命令之后会进入文本编辑器中 --- 新的在后面

```sh
pick 8b485bb add 4
pick a75ed74 add 5

# Rebase 63ce9fb..a75ed74 onto 63ce9fb (2 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

前两条命令就是我们选中的 commit ,

- 我们可以通过移动来修改他们的顺序，
- drop 就是放弃commit
- `reword` 只修改提交的信息，不修改内容
- `edit`  修改提交的内容
	- 会自动到需要修改的地方去
	- 使用 `git rebase --continue`  进入下一步操作
	- 需要解决冲突等问题
- `squash`  commit 合并 --- 合并到最新的
- `fixup`  合并到老版本