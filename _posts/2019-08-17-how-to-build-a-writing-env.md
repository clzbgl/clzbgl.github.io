---
layout: post
title:  "搭建一个写作环境"
date:   2019-08-17 12:59:18 +0800
categories: tech
---

# 一、搭建一个写作环境

- github
- Markdown编辑器：Typora

# 二、行动起来

## 2.1 注册github账号并创建项目

账号：clzbgl

项目：https://github.com/clzbgl/clzbgl.github.io

注意的地方：

- 账号即用户名clzbgl
- 项目命名规则是：用户名.github.io，所以我的项目名就是：clzbgl.github.io

## 2.2 安装Jekyll

> Jekyll是一款静态网页生成器，基于Ruby开发

### 2.2.1 确认Ruby环境

配置Ruby镜像：https://gems.ruby-china.com/

```shell
ruby -v
gem -v
gem install jekyll
jekyll -v
```

![](/assets/img/2019/08/Jietu20190817-015218.jpg)

### 2.2.2 创建第一个Jekyll项目

```shell
jekyll new myblog
cd myblog
jekyll serve
```

将2.1创建的项目克隆下来，将将myblog中的内容拷贝到github项目中去

```shell
git clone git@github.com:clzbgl/clzbgl.github.io.git
cp -R myblog/* clzbgl.github.io/
```

## 2.3 配置Jekyll

### 2.3.1 永久链接

http://jekyllcn.com/docs/permalinks/

### 2.3.2 引用图片

![](/assets/img/2019/08/jekyll-img-solution.png)

### 2.3.3 日期格式

参考：

- [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
- [理想的写作环境：Git+Github+Markdown+Jekyll](http://www.yangzhiping.com/tech/writing-space.html)
- [安装-Jekyll-官方文档](http://jekyllcn.com/docs/installation/ )
- [jekyll操作记录-大漠穷秋](https://damoqiongqiu.github.io/)

