---
layout: post
title: "win python3 安装pysqlcipher3"
date: 2023-10-07
tags: win
description: ""
---

# 安装pysqlcipher3

## 正文

环境：win10,python3.9.5

发现pysqlcipher3>=1.0.3装不上

考虑手动编译

下载 【https://github.com/sqlcipher/sqlcipher】 ，需要手动编译。

下载 vs，安装->使用C++的桌面开发。大约需要10g。下载地址【https://visualstudio.microsoft.com/zh-hans/downloads/】

下载openssl，选msi，安装。打开powershell，输入openssl，没有报错说明环境配置成功。  
如果没有msi的版本，就只能自己编译安装。参考连接【https://www.cnblogs.com/jiaheshang/p/15098075.html】  
安装 Strawberry Perl 下载地址（http://strawberryperl.com/）  
安装完成后，可在cmd界面输入 perl -v 查看是否安装成功  
安装 NASM 下载地址（https://www.nasm.us/）  
安装完成后，将nasm配置在系统环境变量中，可在cmd界面输入 nasm -v 查看是否配置成功  
```
perl Configure VC-WIN64A
nmake test
nmake install
```

安装OpenSSL的时候选择，bin目录，需要将bin目录加入系统PATH中。

tcl 工具安装。,在powershell的终端下输入tchsh，验证tcl是否安装成功。下载地址【https://www.activestate.com/products/tcl/】


## 2.开始编译

```
python setup.py build_amalgamation
没有报错的话，直接安装
python setup.py install
```
### 2.1 报错 - SQL Cipher amalgamation not found.

python setup.py build_amalgamation 后 

报错

```
~ python setup.py build_amalgamation
running build_amalgamation
Builds a C extension using a sqlcipher amalgamation
SQL Cipher amalgamation not found. Please download or build the
        amalgamation and make sure the following files are present in the
        amalgamation folder: sqlite3.h, sqlite3.c
```
这个是因为需要一个.c 和 .h 的文件

### 2.2 下面编译sqlcipher获取.c .h 文件 

安装完OpenssL，安装完Tcl，VisualStudio 社区版就行，并安装“使用c++的桌面开发”，

下载sqlcipher
```
git clone https://github.com/sqlcipher/sqlcipher.git
```

从开始菜单找到 Developer PowerShell for VS 2022，打开
然后切换到sqlcipher 的源码路径下，执行
```
nmake /f Makefile.msc
```

过程如下：
```bash
Microsoft (R) 程序维护实用工具 14.16.27026.1 版
版权所有 (C) Microsoft Corporation。  保留所有权利。
        copy .\tool\lempar.c .
已复制         1 个文件。
        cl -nologo -W4   -MT -DNDEBUG -D_CRT_SECURE_NO_DEPRECATE -D_CRT_SECURE_NO_WARNINGS -D_CRT_NONSTDC_NO_DEPRECATE -D_CRT_NONSTDC_NO_WARNINGS -O2 -Zi -wd4054 -wd4055 -wd4100 -wd4127 -wd4130 -wd4152 -wd4189 -wd4206 -wd4210 -wd4232 -wd4305 -wd4306 -wd4702 -wd4706 -Daccess=_access  -Felemon.exe .\tool\lemon.c /link /DEBUG
lemon.c
        del /Q parse.y parse.h parse.h.temp 2>NUL
        copy .\src\parse.y .
已复制         1 个文件。
        .\lemon.exe  -DSQLITE_MAX_TRIGGER_DEPTH=100  -DSQLITE_ENABLE_FTS3=1 -DSQLITE_ENABLE_RTREE=1 -DSQLITE_ENABLE_GEOPOLY=1 -DSQLITE_ENABLE_JSON1=1 -DSQLITE_ENABLE_STMTVTAB=1 -DSQLITE_ENABLE_DBPAGE_VTAB=1 -DSQLITE_ENABLE_DBSTAT_VTAB=1 -DSQLITE_INTROSPECTION_PRAGMAS=1 -DSQLITE_ENABLE_DESERIALIZE=1 -DSQLITE_ENABLE_COLUMN_METADATA=1   parse.y
        move parse.h parse.h.temp
移动了         1 个文件。
        tclsh .\tool\addopcodes.tcl parse.h.temp > parse.h
        type parse.h .\src\vdbe.c | tclsh .\tool\mkopcodeh.tcl > opcodes.h
parse.h
.\src\vdbe.c
        tclsh .\tool\mkopcodec.tcl opcodes.h > opcodes.c
        cl -nologo -W4   -MT -DNDEBUG -D_CRT_SECURE_NO_DEPRECATE -D_CRT_SECURE_NO_WARNINGS -D_CRT_NONSTDC_NO_DEPRECATE -D_CRT_NONSTDC_NO_WARNINGS -O2 -Zi -wd4054 -wd4055 -wd4100 -wd4127 -wd4130 -wd4152 -wd4189 -wd4206 -wd4210 -wd4232 -wd4305 -wd4306 -wd4702 -wd4706 -Femkkeywordhash.exe  -DSQLITE_MAX_TRIGGER_DEPTH=100  -DSQLITE_ENABLE_FTS3=1 -DSQLITE_ENABLE_RTREE=1 -DSQLITE_ENABLE_GEOPOLY=1 -DSQLITE_ENABLE_JSON1=1 -DSQLITE_ENABLE_STMTVTAB=1 -DSQLITE_ENABLE_DBPAGE_VTAB=1 -DSQLITE_ENABLE_DBSTAT_VTAB=1 -DSQLITE_INTROSPECTION_PRAGMAS=1 -DSQLITE_ENABLE_DESERIALIZE=1 -DSQLITE_ENABLE_COLUMN_METADATA=1    .\tool\mkkeywordhash.c /link /DEBUG
mkkeywordhash.c
        .\mkkeywordhash.exe > keywordhash.h
        tclsh .\tool\mkshellc.tcl > shell.c
        cl -nologo -W4   -MT -DNDEBUG -D_CRT_SECURE_NO_DEPRECATE -D_CRT_SECURE_NO_WARNINGS -D_CRT_NONSTDC_NO_DEPRECATE -D_CRT_NONSTDC_NO_WARNINGS -O2 -Zi -wd4054 -wd4055 -wd4100 -wd4127 -wd4130 -wd4152 -wd4189 -wd4206 -wd4210 -wd4232 -wd4305 -wd4306 -wd4702 -wd4706 -Femksourceid.exe .\tool\mksourceid.c /link /DEBUG
mksourceid.c
        tclsh .\tool\mksqlite3h.tcl . > sqlite3.h
        copy .\ext\fts5\fts5parse.y .
已复制         1 个文件。
        del /Q fts5parse.h 2>NUL
        .\lemon.exe  -DSQLITE_MAX_TRIGGER_DEPTH=100  -DSQLITE_ENABLE_FTS3=1 -DSQLITE_ENABLE_RTREE=1 -DSQLITE_ENABLE_GEOPOLY=1 -DSQLITE_ENABLE_JSON1=1 -DSQLITE_ENABLE_STMTVTAB=1 -DSQLITE_ENABLE_DBPAGE_VTAB=1 -DSQLITE_ENABLE_DBSTAT_VTAB=1 -DSQLITE_INTROSPECTION_PRAGMAS=1 -DSQLITE_ENABLE_DESERIALIZE=1 -DSQLITE_ENABLE_COLUMN_METADATA=1   fts5parse.y
        tclsh .\ext\fts5\tool\mkfts5c.tcl
        copy .\ext\fts5\fts5.h .
已复制         1 个文件。
        rmdir /Q/S tsrc 2>NUL
        mkdir tsrc
        for %i in (.\src\crypto.c  .\src\crypto_cc.c  .\src\crypto_impl.c  .\src\crypto_libtomcrypt.c  .\src\crypto_openssl.c  .\src\crypto.h  .\src\sqlcipher.h  .\src\alter.c  .\src\analyze.c  .\src\attach.c  .\src\auth.c  .\src\backup.c  .\src\bitvec.c  .\src\btmutex.c  .\src\btree.c  .\src\build.c  .\src\callback.c  .\src\complete.c  .\src\ctime.c  .\src\date.c  .\src\dbpage.c  .\src\dbstat.c  .\src\delete.c  .\src\expr.c  .\src\fault.c  .\src\fkey.c  .\src\func.c  .\src\global.c  .\src\hash.c  .\src\insert.c  .\src\legacy.c  .\src\loadext.c  .\src\main.c  .\src\malloc.c  .\src\mem0.c  .\src\mem1.c  .\src\mem2.c  .\src\mem3.c  .\src\mem5.c  .\src\memdb.c  .\src\memjournal.c  .\src\mutex.c  .\src\mutex_noop.c  .\src\mutex_unix.c  .\src\mutex_w32.c  .\src\notify.c  .\src\os.c  .\src\os_unix.c  .\src\os_win.c) do copy /Y %i tsrc
......略
NMAKE : fatal error U1077: “"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\bin\HostX64\x64\cl.EXE"”: 返回代码“0x2”
Stop.
```

**最终编译会报错，不过没关系，我们这里并不需要真正的编译，只需要中间生成的sqlite3.h和sqlite3.c就够了。**

中间过程如果执行有其他差错，可以clean一下再重试
```
nmake /f Makefile.msc clean
```

最后将.c 和 .h文件放到 amalgamation 文件夹下

在pysqlcipher3目录下创建amalgamation目录，拷贝上一步 sqlcipher 项目目录中生成的sqlite3.h和sqlite3.c文件到其中。并在其下创建一个sqlcipher目录，且再次拷贝一份sqlite3.h文件到sqlcipher目录中。
```
mkdir -p /path/to/your/project/amalgamation
cp sqlite3.c /path/to/your/project/amalgamation/
cp sqlite3.h /path/to/your/project/amalgamation/
cp sqlite3.h /path/to/your/project/amalgamation/sqlcipher/
```

## 3.大多时候编译肯定都有问题
### 3.1 报错 - Fatal error: OpenSSL could not be detected!

```
running build_amalgamation
Builds a C extension using a sqlcipher amalgamation
Fatal error: OpenSSL could not be detected!
```

解决方法：

```
添加环境遍历OPENSSL_CONF，值是 “安装位置\OpenSSL-Win64\bin\openssl.cfg”
```

### 3.2. 报错 - LNK1181: 无法打开输入文件“libeay32.lib”

从 1.1.0 版本开始，OpenSSL 将它们的库名称从: libeay32.dll -> libcrypto.dll ssleay32.dll -> libssl.dll
版本问题，改名了。

解决：文件名称在 setup.py 代码写的，改成 libcrypto.lib 可以实现（注意是lib文件）

```
# Configure the linker
ext.extra_link_args.append("libeay32.lib")
ext.extra_link_args.append('/LIBPATH:' + openssl_lib_path)
改为：
# Configure the linker
# ext.extra_link_args.append("libeay32.lib") # by zjx, 因为openssl文件库改名了
ext.extra_link_args.append("libcrypto.lib")
ext.extra_link_args.append('/LIBPATH:' + openssl_lib_path)
```
如果改了后还是找不到，就自己打开openssl的安装路径看看，这个lib文件哪，然后在
```
ext.extra_link_args.append('/LIBPATH:' + openssl_lib_path + "/path/to/lib/")
```
加上后面的路径，保证能找到这个lib文件就行。

### 3.3. 报错 - failed with exit code 2

报错内容：

```
src\python3\cache.c(261): error C2017: 非法的转义序列
src\python3\cache.c(261): error C2061: 语法错误: 标识符“Node”
src\python3\cache.c(261): error C2001: 常量中有换行符
src\python3\cache.c(303): error C2017: 非法的转义序列
src\python3\cache.c(303): error C2224: “.Cache”的左侧必须具有结构/联合类型
src\python3\cache.c(303): error C2001: 常量中有换行符
src\python3\cache.c(303): error C2059: 语法错误:“字符串”
error: command 'D:\\software\\Microsoft Visual Studio\\Community2022\\VC\\Tools\\MSVC\\14.36.32532\\bin\\HostX86\\x64\\cl.exe' failed with exit code 2
```

报错文件指向src\python3\cache.c打开看看：

```
# 分析报错文件，MODULE_NAME有问题
261        MODULE_NAME "Node",                             /* tp_name */
303        MODULE_NAME ".Cache",                           /* tp_name */
```

宏MODULE_NAME在编译前没有进行正确的分析，就是这个不能用

分析py文件看看 MODULE_NAME 是个啥。

```
define_macros = [('MODULE_NAME', quote_argument(PACKAGE_NAME + '.dbapi2'))]
print(define_macros) # 搞一个输出看看是啥 我的是 pysqlcipher3.dbapi2
```

分析py文件发现，MODULE_NAME 的值应该是 ```"pysqlcipher3.dbapi2"```

将src\python3\cache.c文件中的MODULE_NAME替换成```"pysqlcipher3.dbapi2"```试试

新的报错

```
src\python3\connection.c(1546): warning C4090: “=”: 不同的“const”限定符
src\python3\connection.c(1697): error C2017: 非法的转义序列
```

查看报错文件(1546 是个告警)，发现还是```MODULE_NAME```

```
1697        MODULE_NAME ".Connection",                      /* tp_name */
```

改了MODULE_NAME在尝试，最后发现src\python3*.c 几乎都用了MODULE_NAME，都改成```"pysqlcipher3.dbapi2"```就可以了。

```
将
MODULE_NAME "Node",   
改为：
"pysqlcipher3.dbapi2" "Node",  
```

最后就编译成功。

执行：```python setup.py install``` 安装成功。

## 最后

 虚拟环境如果想安装:

先把虚拟环境激活，然后用 带着虚拟环境的命令行 进行 install命令 就可以了。

例如：

```
 source xxx/venv/Scripts/activate
 (venv) cd /path/to/pysqlciher3/
 (venv) python setup.py install
```

**参考连接：[点这里](https://www.zybuluo.com/caipiz/note/1396788)**