+++
date = "2017-04-05T21:16:33+08:00"
title = "从hexo迁移到hugo"
description = ""
topics = ["杂项"]
tags = []
draft = false

+++

我的博客最初是基于hexo生成的静态博客，前段时间重新安装系统，写博客时发现电脑没有安装node环境，于是决定用hugo来生成文章。
<!--more-->
hugo总的来说有两个特点：

- 安装方便，只需一个二进制可执行文件
- 生成速度快

## 需要注意的地方

### Hugo 的配置文件是 TOML 格式的，其生成的 Markdown 文件的 front matter（不知如何翻译）也是 TOML 结构：
```
+++
date = "2016-09-30T17:14:37+08:00"
description = ""
title = "Migrate to Hugo from Hexo"
tags = ["hugo", "hexo", "blog"]

+++
```

而 Hexo 的配置文件是 YAML 格式，其 Markdown 文件的 front matter 如下：
```
title: 'Start React with Webpack'
date: 2016-09-09 04:11:13
tags:
  - webpack
  - react

---
```

### data的格式