---
title: windows中删除超长的node_modules
date: 2016-10-11 13:41:55
tags: [Node.js]
categories: 杂项
---
在Node.js开发中，依赖第三方项目，会产生比较深的文件目录，Windows下无法简单删除

## 解决方法
* 新建空白目录，如`F:\git\temp`
* 管理员方式打开命令行窗口
* 输入`robocopy F:\git\temp F:\git\test\node_modules /purge`
* 回车，搞定

更多关于robocopy的说明请看 [robocopy](http://baike.baidu.com/link?url=6XgB5Fwho9P1p16Vrn8jHOSoNG6RDZgqiEHm6TkPsP3GzO6UsHUiFHPje3ZRmeDgDxilyQx9x9aAJz0nBOzYYeuyselIEwVkfDZX_KRJGaC)