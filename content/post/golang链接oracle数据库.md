+++
topics = ["golang"]
tags = ["golang","oracle"]
draft = false
date = "2017-11-02T11:12:46+08:00"
title = "golang链接oracle数据库"

+++

本文记录golang使用oci链接Oracle数据库
<!--more-->
## 系统参数
* windows10 64 专业版 1709
* Instant Client Package - Basic 12.2.0.1.0 [Oracle
  Instant Client](http://www.oracle.com/technetwork/database/features/instant-client/index.html)
* Instant Client Package - SDK 12.2.0.1.0 [Oracle
  Instant Client](http://www.oracle.com/technetwork/database/features/instant-client/index.html)
* MinGW-w64 - for 32 and 64 bit Windows [MinGW](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/7.2.0/threads-posix/seh/)
* pkg-config [pkg-config](http://ftp.gnome.org/pub/gnome/binaries/win64/dependencies/)
* libglib-2.0-0.dll

## 系统变量设置
* path: `C:\mingw64\bin;C:\instantclient_12_2`
* PKG_CONFIG_PATH: `C:\pkgconfig`

## 在`C:\pkgconfig`下新建文件:oci8.pc
```
prefix=C:/instantclient_12_2
libdir=${prefix}
includedir=${prefix}/sdk/include/

Name: OCI
Description: Oracle database engine
Version: 11.2
Libs: -L${libdir} -loci
Libs.private:
Cflags: -I${includedir}
```

## 安装go-oci8
```bash
go get github.com/mattn/go-oci8
```

## 编写代码
```
package main
    
import (
    "fmt"
    "database/sql"
    _ "github.com/mattn/go-oci8"
)
    
func main(){
    db, err := sql.Open("oci8", "username/password@localhost:1521/xe")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer db.Close()
    
    if err = db.Ping(); err != nil {
        fmt.Printf("Error connecting to the database: %s\n", err)
        return
    }
    
    rows,err := db.Query("select 2+2 from dual")
    if err != nil {
        fmt.Println("Error fetching addition")
        fmt.Println(err)
        return
    }
    defer rows.Close()
    
    for rows.Next() {
        var sum int
        rows.Scan(&sum)
        fmt.printf("2 + 2 always equals: %d\n", sum)
    }
}
```