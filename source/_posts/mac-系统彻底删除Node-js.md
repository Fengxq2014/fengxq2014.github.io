---
title: mac 系统彻底删除Node.js
date: 2016-08-01 14:21:08
tags: [Node.js]
categories: 杂项
---
首先创建一个shell文件
```bash
touch uninstall_node.sh
```
<!-- more -->
然后将下面一段脚本拷贝到文件中，保存退出
```bash
#!/bin/bash
lsbom -f -l -s -pf /var/db/receipts/org.nodejs.pkg.bom \
| while read i; do
sudo rm /usr/local/${i}
done
sudo rm -rf /usr/local/lib/node \
   /usr/local/lib/node_modules \
   /var/db/receipts/org.nodejs.*
```

赋予文件执行权限
```bash
chmod 777 uninstall_node.sh
```

最后执行脚本
```bash
sh uninstall_node.sh
```