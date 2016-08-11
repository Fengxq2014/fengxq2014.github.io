---
title: Node.js入门-NPM与包
tags:
  - Node.js
  - Node.js入门
categories: Node.js
date: 2016-03-29 11:09:45
---

Node.js组织了自身的核心模块，也使得第三方文件模块可以有序的编写和使用。但是第三方模块中，模块与模块之间仍然是散列在各地的，互相之间不能直接引用。而在模块之外，包和NPM则是将模块联系起来的一种机制。

在介绍NPM之前，不得不提起CommonJs的包规范。JavaScript不似Java或者其他语言那样，具有模块和包机构。Node.js对模块规范的实现，一定程度上解决了变量依赖、依赖关系等代码组织性问题。包的出现，则是在模块的基础上进一步组织JavaScript代码。
<!-- more -->
CommonJs的包规范的定义其实也十分简单，它是由包结构和包描述文件两部分组成，前者用于组织包中的各种文件，后者用于描述包的相关信息，以供外部读取分析。

## 包结构
包实际上是一个存档文件，即一个目录直接打包为.zip或者tar或tar.gz格式的文件，安装后解压还原为目录，完全符合CommonJs规范的包目录应该包含如下这些文件
>* package.json：包描述文件
* bin：用于存放可执行二进制文件
* lib：用于存放JavaScript代码的目录
* doc：用于存放文档的目录
* test：用于存放单元测试用例的代码
 
可以看到，CommonJs包规范从文档、测试等方面都做过考虑。当一个包完成后向外公布时，用户看到单元测试和文档的时候会給他们一种踏实可靠的感觉。

## 包描述文件与NPM
包描述文件用于表达非代码相关的信息，它是一个json格式的文件--package.json，位于包的根目录下，是包的重要组成部分。而NPM的所有行为都与包描述文件的字段信息息息相关。由于CommonJs包规范尚处于草案阶段，NPM在实践中做了一点的取舍，具体细节在后面会介绍到。

在包描述文件的规范中，NPM实际需要的字段主要有name，version，description，keywords，repositories，author，bin，main，scripts，engines，dependencies，devDependencies，contributors。
>* name：包名，规范定义它需要由小写的字母和数字组成，可以包含.、-和_，但不允许出现空格。包名必需唯一，以免对公布时产生重名冲突，除此之外，NPM还建议不要在包名中带上node或js来重复标识它是JavaScript或Node.js模块。
* description：包简介
* version：版本号，一个语义化的版本号，这在<http://semver.org>上有详细定义，通常为major.minor.revision格式
* keyword：关键字数组，NPM中主要用来做分类搜索，一个好的关键词数组有利于用户快速找到的编写的包
* repositories：托管源代码的位置列表，表明可以通过哪些方式和地址访问包的源代码
* author：包作者
* bin：一些包作者希望包可以作为命令行工具使用。配置好bin字段后，通过npm install package_name -g命里可以将脚本添加到执行路径中，之后可以在命里行中直接执行。
* main：模块引入方法require()在引入包时，会优先检查这个字段，并将其作为包中其余模块的入口，如果不存在这个字段，require()方法就会查找包目录下的index.js、index.node、index.json文件作为默认入口
* scripts：脚本说明对象。它主要被包管理器用来安装、编译、测试、和卸载包。示例如下：
 ```json
 "scripts":{
     "install":"install.js",
     "uninstall":"uninstall.js",
     "build":"build.js",
     "doc":"make-doc.js",
     "test":"test.js"
 }
 ```
* engines：支持的JavaScript引擎列表，有效的引擎取值包括ejs、flusspferd、gpsee、jsc、spidermonkey、narwhal、node和v8
* dependencies：使用当前包所需要依赖的包列表。这个属性十分重要，NPM会通过这个属性帮助自动加载依赖的包。
* devDependencies：一些模块只是在开发时需要依赖。配置这个属性，可以提示包的后续开发者安装依赖包
* contributors：贡献者列表。在开源社区中，为开源项目提供代码是经常出现的事情，如果名字能出现在知名项目的contributors列表中，是一件比较有荣誉感的事。列表中第一个贡献应当是包的作者本人

## NPM常用功能
CommonJs包规范是理论，NPM是其中一种实践。NPM之于Node.js，相当于gem之于Ruby，pear之于PHP。对于Node.js而言，NPM帮助完成了第三方模块的发布、安装和依赖等。借助NPM，Node.js与第三方模块之间形成了很好的一个生态系统。

借助NPM，可以帮助用户快速安装和管理依赖包。除此之外，NPM还有一些巧妙的用法

### 查看帮助
安装Node.js之后，执行npm -v命令可以查看当前NPM的版本
![](http://7xqe60.com1.z0.glb.clouddn.com/npm-v.png)

在不熟悉npm命令之前，可以直接执行npm查看到帮助引导说明
![](http://7xqe60.com1.z0.glb.clouddn.com/npm-help.png)
可以看到，帮助中列出了所有的命令，其中npm help &lt;command&gt;可以查看具体的命令说明。

### 安装依赖包
安装依赖包是npm最常见的用法，它的执行语句是
```bash
npm install package_name

# 如：
npm install express
```
执行该语句后，npm会在当前目录下创建node_modules目录，然后在node_modules目录下创建express目录，接着将包解压到这个目录下。

安装号依赖包后，直接在代码中调用require('express');即可引入改包。require()方法在做路径分析的时候会通过模块路径查找到express所在的位置。模块引入和包的安装这两个步骤是相辅相成的。
#### 全局安装模式
如果包中含有命令行工具，那么需要执行
```bash
npm install package_name -g

# 如：
npm install express -g
```
需要注意的是，全局模式并不是将一个模块包安装为一个全局包的意思，它并不意味着可以从任意地方通过require()来引用到它。

全局模式这个称谓其实并不精确，存在诸多误导。实际上，-g是将一个包安装为全局可执行命令。它根据包描述文件中的bin字段配置，将实际脚本链接到与Node.js可执行文件相同的路径下：
```json
"bin":{
    "express":"./bin/express"
}
```
#### 从本地安装
对于一些没有发布到npm上的包，或是因为网络原因导致无法直接安装的包，可以通过将包下载到本地，然后以本地安装。本地安装只需要为npm指明package.json文件所在的位置即可：它可以是一个包含package.json的存档文件，也可以是一个url地址，也可以是一个目录下有package.json文件的目录位置。
* npm install &lt;tarball file&gt;
* npm install &lt;tarball url&gt;
* npm install &lt;folder&gt;

#### 从非官方源安装
如果不能通过官方源安装，可以通过镜像安装，在执行命令时，添加--registry=http://registry.url即可

如果使用过程中几乎都采用镜像源安装，可以执行以下命令指定默认源：
```bash
npm config set registry http://registry.url
```