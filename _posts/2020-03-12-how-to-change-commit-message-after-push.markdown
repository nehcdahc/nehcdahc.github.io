---
layout: post
title: "如何修改已经推送的 git commit messages"
date: 2020-03-12 21:33:00 +0800
tags: [Git]
categories: ["工具"]
---

## 步骤

1. 查看历史修改

   ```bash
   git log
   ```

1. 使用 git 默认的编辑器显示当前分支的最近 n 个 commit 的 messages

   ```bash
   # git rebase -i HEAD~{n}
   git rebase -i HEAD~3 # Displays a list of the last 3 commits on the current branch
   ```

   结果示例

   ```txt
   pick e499d89 Delete CNAME
   pick 0c39034 Better README
   pick f7fde4a Change the commit message but push the same commit.

   # Rebase 9fdb3bd..f7fde4a onto 9fdb3bd
   #
   # Commands:
   # p, pick = use commit
   # r, reword = use commit, but edit the commit message
   # e, edit = use commit, but stop for amending
   # s, squash = use commit, but meld into previous commit
   # f, fixup = like "squash", but discard this commit's log message
   # x, exec = run command (the rest of the line) using shell
   #
   # These lines can be re-ordered; they are executed from top to bottom.
   #
   # If you remove a line here THAT COMMIT WILL BE LOST.
   #
   # However, if you remove everything, the rebase will be aborted.
   #
   # Note that empty commits are commented out
   ```

1. 使用 reword 或者 edit 替换每个提交消息之前的 pick

   ```
   pick e499d89 Delete CNAME

   reword 0c39034 Better README
   edit f7fde4a Change the commit message but push the same commit.
   ```

1. 保存并关闭 commit 列表文件

1. 在每一个 commit 文件写上新的 commit message，然后保存关闭。注意：如果有使用 edit 的行，在编译完成之后还要执行下列命令。

   ```bash
   git commit --amend
   git rebase --continue
   ```

1. 强制提交追加的 commits

   ```bash
   git push -f
   ```

## 参考

- <https://help.github.com/en/github/committing-changes-to-your-project/changing-a-commit-message>
- <https://blog.csdn.net/qq_39956074/article/details/83992286>
