---
title: Mac下Github+hexo搭建静态博客
date: 2016-06-01 11:23:47
tags: hexo
---

基本参照了下面这个链接搭建的: http://www.jianshu.com/p/70974a67ec8f

部署部分做了如下的修改：
```
deploy:
  type: git
  #repository: http://github.com/alexkkk/alexkkk.github.io.git
  repository: git@github.com:alexkkk/alexkkk.github.io.git
  branch: master
```



