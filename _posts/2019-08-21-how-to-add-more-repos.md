---
layout: post
title:  "Git如何添加多个远程仓库"
date:   2019-08-21 20:57:00 +0800
categories: tech
---



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

