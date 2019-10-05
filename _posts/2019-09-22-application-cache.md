---
layout: post
title:  "[未完成]应用缓存"
date:   2019-09-22 19:51:42 +0800
categories: tech
typora-root-url: ..
---

缓存让数据更接近使用者目的是让访问速度更快

缓存哪些数据？

缓存指标：缓存命中率

缓存回收策略？

缓存回收算法？

Java缓存类型?



堆缓存

- Gauva Cache实现

- Ehcache 3.x实现

- MapDB 3.x实现

堆外缓存

- Ehcache 3.x实现
- MapDB 3.x实现

磁盘缓存

- Ehcache 3.x实现
- MapDB 3.x实现

分布式缓存

- Ehcache 3.1 + Terracotta server
- Redis

多级缓存

应用缓存示例

缓存使用模式

- Cache-Aside，业务代码维护缓存
- Cache-As-SoR，所有操作都对缓存操作，三种实现
  - Read-Through，
  - Write-Through，
  - Write-Behind，
- Copy Pattern

性能测试





 

