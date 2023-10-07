---
title: "Github启用email保护"
date: "2023-10-07T15:36:44+08:00"
draft: false
tags: ["git"]
topics: ["杂项"]
---

## 开启github email保护

在[github settings](https://github.com/settings/emails)中可以开启 `Keep my email addresses private` 和 `Block command line pushes that expose my email` 两个选项

如果此时使用使用命令push代码时会收到以下报错：
```bash
remote: error: GH007: Your push would publish a private email address.
remote: You can make your email public or disable this protection by visiting:
remote: http://github.com/settings/emails
```

## 解决方法如下

* 在[github settings](https://github.com/settings/emails)中找到GitHub noreply address
* 修改git仓库email地址

    ```bash
    # 修改当前仓库
    git config user.email "{ID}+{username}@users.noreply.github.com"

    # 修改所有仓库
    git config --global user.email "{ID}+{username}@users.noreply.github.com"
    ```
* 重置最后一次提交中的作者信息
    ```bash
    git commit --amend --reset-author --no-edit
    ```
* 最后正常提交即可
    ```bash
    git push
    ```