---
title: git如何删除commit点
categories: log
date: 2018-02-09 11:22:17
tags: Git
---

分为两种情况
1. 只删除commit信息，不删除内容 `git reset HEAD~1`
2. 删除commit信息包括内容 `git reset --hard HEAD~1`

`HEAD~1`是要回退到的点，需要回退到想要删除到`commit`点的上一个点，回退后继续 `git add` && `git commit`等常规操作即可

如果还需要更新远程库，直接`git push`时会失败，提示说我们本地版本落后远程库的版本，这时可以使用参数`--force`，即`git push --force`

需要注意的是，`--force`是个危险参数，使用后会用当前的版本替换远程版本，除非你确定需要这么做，否则不要轻易使用该参数