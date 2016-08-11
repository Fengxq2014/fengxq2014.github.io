---
title: Git忽略规则及.gitignore规则不生效的解决办法
date: 2016-05-24 14:11:18
tags: [git]
categories: 杂项
---
利用.gitignore过滤文件，如编译过程中的中间文件，等等，这些文件不需要被追踪管理。
但是，当项目一开始没有添加.gitignore文件，后来添加发现忽略规则不生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：
```bash
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```