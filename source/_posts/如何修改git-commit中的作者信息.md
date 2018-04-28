---
title: 如何修改git commit中的作者信息
categories: log
date: 2018-04-28 22:59:03
tags: Git
---

如果`git commit`提交信息前没有设置正确的`user.name` `user.email`, git commit之后如何修改提交信息中的作者信息？

分两种情况

### 第一种情况  

修改最近一次commit中的作者信息

1.设置正确的作者信息

```bash
# 设置名称为 zhangxiaoshang
git config --global user.name zhangxiaoshang
# 设置邮箱为 zhangxiaoshang66@163.com
git config --global user.email zhangxiaoshang66@163.com
```

2.修改commit中的作者信息

```bash
git commit --amend --reset-author
```

然后会弹出编辑窗口, 直接输入 `:wq` 退出即可

### 第二种情况

如果你是想修改最后一次之前的commit中的作者信息

比如，如果你的提交历史是 `hashA-hashB-hashC-hashD-hashE-hashF`,其中 `hashF`是最后一次提交,如果你想修改的`hashC`提交中的作者信息,你需要：

1.执行 `git rebase -i hashB`[(这里有一个示例：执行这条命令后你将看到类似这样的信息)](https://help.github.com/articles/about-git-rebase/#an-example-of-using-git-rebase)

* 如果你需要修改的是`hashA` 使用 `git rebase -i --root`

2.将 `hashC` 这一行的 `pick` 改为 `edit`

3.执行 `git commit --amend --author="zhangxiaoshang <zhangxiaoshang66@163.com>"` (这里将作者用户名改为了：zhangxiaoshang 邮箱改为：zhangxiaoshang66@163.com)

4.然后 `git rebase --continue`

以上

第二种情况的解决方案参考 [Stack Overflow](https://stackoverflow.com/questions/3042437/change-commit-author-at-one-specific-commit)(可能需要科学上网)