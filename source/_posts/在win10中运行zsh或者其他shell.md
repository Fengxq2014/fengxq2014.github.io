---
title: 在win10中运行zsh或者其他shell
date: 2016-08-11 10:36:04
tags: [Windows]
categories: 杂项
---

win10周年更新之后Windows原生支持了bash shell，本文讨论如何在win10下运行zsh shell或者其他shell，关于win10 bash shell的安装，请自行谷歌。
<!-- more -->
## 安装Zsh
当你安装Bash之后，在cmd中输入`bash`即可进入Ubuntu环境，之后使用`apt-get`安装Zsh：
```bash
sudo apt-get install zsh
```
根据提示安装成功之后，输入
```bash
zsh
```
即可调用zsh
通过下面的命令退出zsh，返回bash
```bash
exit
```

## 进入bash时自动切换zsh
修改.bashrc文件即可，将下面的代码添加到`~/.bashrc`文件中
```bash
# Launch Zsh
if [ -t 1 ]; then
exec zsh
fi
```

## 遇到的问题
### 进入shell之后之前安装的nvm找不到了
在~/.zshrc中添加：
```
source ~/.nvm/nvm.sh
```