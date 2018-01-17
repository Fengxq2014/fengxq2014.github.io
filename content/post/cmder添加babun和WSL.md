+++
date = "2017-04-14T16:40:47+08:00"
title = "cmder添加babun和WSL"
description = ""
tags = ["Windows"]
topics = ["杂项"]
draft = false

+++

## babun 
* Task parameters:
```bash
/icon "C:\Users\DefNed\.babun\cygwin\bin\mintty.exe" /dir "C:\Users\DefNed\"
```

<!--more-->
* Commands:
```bash
C:\Users\DefNed\.babun\cygwin\bin\mintty.exe /bin/env CHERE_INVOKING=1 /bin/zsh.exe
```

## WSL(windows subsystem for linux)并使用zsh
* Task parameters:
```bash
/icon  /icon "%CMDER_ROOT%\icons\cmder.ico"
```

* Commands:
```bash
C:\Windows\System32\bash.exe -c "/usr/bin/zsh"
```