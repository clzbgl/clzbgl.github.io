---
layout: post
title:  "Git如何添加多个远程仓库"
date:   2019-08-21 20:57:00 +0800
categories: tech
---

# git如何添加多个远程仓库

```shell
git remote add origin git@github.com:clzbgl/clzbgl.github.io.git
git remote add oschina git@gitee.com:clzbgl/clzbgl.git

# 默认origin库，master分支
git push
git push oschina

# 一次提交
git push --all
```

理解pull与push的参数

```shell
git pull 是 git pull (from) origin (to) master
git push 是 git push (to) origin (from) master
```

遇到了一个错误

```shell
fatal: refusing to merge unrelated histories
其实这个问题是因为两个根本不相干的 git 库，一个是本地库，一个是远端库，然后本地要去推送到远端，远端觉得这个本地库跟自己不相干，所以告知无法合并

git pull oschina master --allow-unrelated-histories
后面加上 --allow-unrelated-histories， 把两段不相干的 分支进行强行合并
后面再push就可以了 git push oschina master:init
```

