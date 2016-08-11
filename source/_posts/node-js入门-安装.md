---
title: Node.js入门-安装
date: 2016-01-26 11:32:51
tags: [Node.js,Node.js入门]
categories: Node.js
---

# 版本控制
由于Node.js版本发布较快，网友称之为“版本帝”，所以一个有效的版本控制管理软件是必不可少的。目前主要的版本控制有nvm和n两种。下面分别介绍：
## nvm&nvmw
 * nvm *Node Version Manager*
 * nvmw是nvm的Windows版本，用于Windows下Node.js版本管理

安装一个nvm<https://github.com/creationix/nvm>
<!--more-->
```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.4/install.sh | bash
```
验证安装
```bash
nvm
```
如果有输出则安装成功，接着安装Node.js,安装最新版：
```bash
nvm install stable

```
安装完之后
```bash
nvm ls
```
![](http://7xqe60.com1.z0.glb.clouddn.com/1.png)
小箭头的意思就是现在正在使用的版本，我这里是 v0.10.29。我还安装了 v0.11.14，但它并非我当前使用的版本。如果你那里没有出现绿色小箭头的话，告诉 nvm 你要使用 0.12.x 版本
```bash
nvm use 0.12
```
然后再次查看，这时候小箭头应该出现了。然后在终端中输入
```bash
node
```
REPL(read–eval–print loop) 应该就出来了，那我们就成功了。

## 完善安装
上述过程完成后，有时会出现，当开启一个新的 shell 窗口时，找不到 node 命令的情况。这种情况一般来自两个原因
一、shell 不知道 nvm 的存在
二、nvm 已经存在，但是没有 default 的 Node.js 版本可用。
解决方式：
一、检查 ~/.profile 或者 ~/.bash_profile 中有没有这样两句
```bash
export NVM_DIR="/Users/YOURUSERNAME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm
```
没有的话，加进去。这两句会在 bash 启动的时候被调用，然后注册 nvm 命令。

二、调用
```bash
nvm ls
```
看看像不像上述图1中一样，有 default 的指向。

如果没有的话，执行
```bash
nvm alias default stable
```
再
```bash
nvm ls
```
看一下

## n
与nvm不同，n是npm的一个全局包，依赖于npm，需要先安装Node.js（npm）。具体安装步骤：
```bash
npm install n -g
```
安装最新的版本
```bash
n latest
```
安装稳定版本
```bash
n stable
```
删除某个版本
```bash
n rm 0.10.1 
```
以指定的版本来执行脚本
```bash
n use 0.10.21 some.js
```

## 非常规升级方法（Windows）
下载官方最新的安装包，覆盖安装即可 ^_^

# 安装
最好在Linux或者mac下开发Node.js，坚持在Windows下开发的同学他们一定有着过人的、甚至是不可告人的兼容性 bug 处理能力。现在Node.js安装包中集成了npm，所以只需安装Node.js，无需单独安装npm。
> 作为一个 Node.js 开发者，如果你还在用 Windows，那你一定是上辈子折翼的天使……（大雾）。Node.js 应用的开发过程需要用到大量的命令行操作，偏偏 Windows 对命令行工具的支持是最薄弱的，并且有相当一部分模块在 Windows 上无法编译通过。 --[淘宝fed](http://taobaofed.org/blog/2015/12/08/efficient-node-dev-cli-setup/)

* Windows
 * 官方网站提供msi和exe安装包，直接下载安装即可 
* Debian and Ubuntu
 ```bash
 curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
 sudo apt-get install -y nodejs
 ```
* 其他发行版见 https://nodejs.org/en/download/package-manager/
* Mac OS X
 * 与Windows一样推荐安装包安装

# 模块
Node.js自2009年发布之后，立即引起了广大前段开发者的共鸣，Node.js发展如此迅速，与其繁荣的社区密不可分。
npm是Node.js的模块管理工具，我们开发所需的大量模块都是npm帮我们安装
# 谁在使用Node.js
* 前后端编程统语言环境统一  
  这类代表是雅虎，雅虎开放了Cockail框架，利用自己深厚的前段沉淀，将YUI3这个前端框架的能力借助Node.js延伸到服务端，使得使用者摆脱了日常工作中一边写JavaScript一边写php所带来的上下文交换负担。
* Node.js带来的高性能I/O用于实时应用  
  Voxer将Node.js应用在实时语音上。国内腾讯的朋友网将Node.js应用在长连接中，以提供实时功能，花瓣网、蘑菇街等公司通过socket.io实现实时通知的功能。
* 并行I/O使得使用者可以更高效地利用分布式环境  
  代表：阿里巴巴和eBay。阿里巴巴的NodeFox和eBay的ql.io都是借用Node.js并行I/O的能力，更高效地使用已有的数据。
* 并行I/O，有效利用稳定接口提升web渲染能力  
  雪球财经和LinkedIn的移动版网站均是这种案例，撇弃同步等待式的顺序请求，大胆采用并行I/O，加速数据获取进而提升web的渲染速度。
* 云计算平台提供Node.js支持  
  微软将Node.js引入Azure的开发中，阿里云、百度云均纷纷在云服务器上提供Node.js应用托管服务，Joyent更是云计算中提供Node.js支持的代表。这类平台看中JavaScript带来的开发上的优势，以及低资源占用、高性能的特点。
* 游戏开发领域  
  游戏领域对实时和并发有很高的要求，网易开源了pomelo实时框架，可以应用在游戏和高实时应用中。
* 工具类  
  过去依赖Java或者其他语言构建的前端工具类应用，纷纷被一些前端工程师用Node.js重写，用前端熟悉的语言为前端构建熟悉的工具，如：Grunt、FIS等。