---
layout: post
title: "安卓微信数据库解密"
date: 2023-08-25
tags: linux
description: ""
---

## 0 背景
时间长了微信聊天记录就非常的长，想保留一些自己的回忆。
尝试在win 和安卓上做相关的功能。

## 1 PC 记录导出
注意：这一节操作在 Windows 平台进行。

### 1.1 数据库位置
好友微信 id 等各种信息存储位置： C:\Users\xxxxxx\Documents\WeChat Files\wxid_xxxxxxxxxx\Msg

![image](/images/posts/2023-08-25/0.png)
数据库位置  

聊天记录存储在 multi 目录下的 MSG0.db： C:\Users\xxxxxx\Documents\WeChat Files\wxid_xxxxxxxxxx\Msg\Multi

![image](/images/posts/2023-08-25/1.png)
聊天记录位置  
### 1.2 获取密钥
可以通过以下代码（GetWeChatAesKey.py）获取密钥，需要修改实际微信的版本与对应的偏移地址：
```
#! /usr/bin/env python3
# -*- coding: utf-8 -*-


import pymem
import struct
import binascii


AESKEY_OFFSET = 0x1DDF914
WECHAT_VERSION = "3.3.0.115"


def getAesKey(p):
    # 获取 WeChatWin.dll 的基地址
    base_address = pymem.process.module_from_name(p.process_handle, "wechatwin.dll").lpBaseOfDll

    # 读取 AES Key 的地址
    result = p.read_bytes(base_address + AESKEY_OFFSET, 4)
    addr = struct.unpack("<I", result)[0]

    # 读取 AES Key
    aesKey = p.read_bytes(addr, 0x20)

    # 解码
    result = binascii.b2a_hex(aesKey)
    return base_address, result.decode()


if __name__ == "__main__":
    print(f"微信版本为：{WECHAT_VERSION}\n密钥偏移地址为：{hex(AESKEY_OFFSET)}")
    p = pymem.Pymem()
    p.open_process_from_name("WeChat.exe")
    base_offset, aesKey = getAesKey(p)
    print(f"数据库密钥为：{aesKey}")
```

![image](/images/posts/2023-08-25/3.png)
获取密钥  
### 1.3 解密
通过以下代码（crack_wechat_msg_pc.py）对数据库进行解密，需要修改对应的密钥：

```
#! /usr/bin/env python3
# -*- coding: utf-8 -*-

import hmac
import ctypes
import hashlib
from Crypto.Cipher import AES


def decrypt_msg(path, password):
    KEY_SIZE = 32
    DEFAULT_ITER = 64000
    DEFAULT_PAGESIZE = 4096  # 4048数据 + 16IV + 20 HMAC + 12
    SQLITE_FILE_HEADER = bytes("SQLite format 3", encoding="ASCII") + bytes(1)  # SQLite 文件头

    with open(path, "rb") as f:
        # TODO: 优化，考虑超大文件
        blist = f.read()

    salt = blist[:16]  # 前16字节为盐
    key = hashlib.pbkdf2_hmac("sha1", password, salt, DEFAULT_ITER, KEY_SIZE)  # 获得Key

    page1 = blist[16:DEFAULT_PAGESIZE]  # 丢掉salt

    mac_salt = bytes([x ^ 0x3a for x in salt])
    mac_key = hashlib.pbkdf2_hmac("sha1", key, mac_salt, 2, KEY_SIZE)

    hash_mac = hmac.new(mac_key, digestmod="sha1")
    hash_mac.update(page1[:-32])
    hash_mac.update(bytes(ctypes.c_int(1)))

    if hash_mac.digest() != page1[-32:-12]:
        raise RuntimeError("Wrong Password")

    pages = [blist[i:i+DEFAULT_PAGESIZE] for i in range(DEFAULT_PAGESIZE, len(blist), DEFAULT_PAGESIZE)]
    pages.insert(0, page1)  # 把第一页补上

    with open(f"{path}.dec.db", "wb") as f:
        f.write(SQLITE_FILE_HEADER)  # 写入文件头

        for i in pages:
            t = AES.new(key, AES.MODE_CBC, i[-48:-32])
            f.write(t.decrypt(i[:-48]))
            f.write(i[-48:])


if __name__ == "__main__":
    path = "MSG0.db"
    key = bytes.fromhex("刚才获取到的密钥")

    decrypt_msg(path, key)
```
至此，我们已经获取了明文数据库，后续根据需求进一步处理，可以使用 DB Browser for SQLite 、 DBeaver 等工具查看。

## 2 手机记录导出
很多时候，PC 上的聊天记录并不全，所以光导出 PC 上的聊天记录可能满足不了需求，还需要导出手机上的记录。这部分介绍如何导出手机上的聊天记录。有超级权限的手机，可跳过备份记录到电脑和恢复记录到模拟器部分；使用模拟器则需要微信号仍能登录。

### 2.1 备份记录到电脑
登录微信 PC 版或者 Mac 版，选择 备份聊天记录到电脑，然后在手机上选择需要备份（导出）的聊天记录，然后等待备份完成。

![聊天记录备份与恢复](/images/posts/2023-08-25/4.png)

### 2.2 恢复记录到模拟器
一方面，大部分手机 root 后会失去保修；另一方面，root 后可能会让手机处于城门大开状态，所以不会轻易 root，于是就通过模拟器曲线救国。

### 2.2.1 安装模拟器
这里选择的是网易的MuMu模拟器[1]，根据系统，下载安装即可。

### 2.2.2 基本设置
安装完成后，需要做一些基本设置。

• Mac 版

点击右上角 系统设置，基本设置，更改或者记住共享文件夹路径，开启ROOT权限，再保存。

![image](/images/posts/2023-08-25/5.jpg)
模拟器设置-Mac

• Windows 版

操作类似，打开 MuMu 模拟器右上角 设置中心 —— 基本设置 —— root权限，选择开启（如果没有该选项，则说明模拟器已默认开启root权限）。可参考《打开“共享文件夹”提示没有ROOT权限》[2]。

![image](/images/posts/2023-08-25/6.png)
模拟器设置-Windows

共享文件可通过主界面下方的 文件共享 打开：

![image](/images/posts/2023-08-25/7.jpg)
文件共享
### 2.2.3 配置文件管理器
点击 文件管理器 左上角的 三，然后点击 齿轮，然后点击 常规设置，然后点击 访问模式，然后选择 超级用户访问模式，选择 永久记住选择，允许，之后会看见切换到了根目录（/）。

![image](/images/posts/2023-08-25/8.png)  
打开超级用户访问模式1  
![image](/images/posts/2023-08-25/9.png)  
打开超级用户访问模式2  
### 2.2.4 安装微信
上面那张图可以看到这次操作里要用到的两个工具：微信 和 文件管理器。其中，文件管理器 是系统自带的，默认在 系统应用 里，我把它拖出来了；微信则需要自己安装。安装也很容易，上图中顶部有个搜索栏，直接在那里输入 微信，回车，就能看到相关的搜索结果，选择 微信，点击下载，就会自己下载安装了。安装完成之后就会出现 打开 按钮，桌面上也会出现 微信。

### 2.2.5 同步聊天记录
一切准备妥当，接下来可以把聊天记录恢复到模拟器（假手机）里了。

**注意**：在模拟器登录微信后，手机上的微信会退出，此时如果有新消息，则会发送到模拟器里。换言之，这些新消息将会“丢失”。故此，建议在月黑风高、夜深人静，没有消息的时候操作。步骤如下：

1. 在模拟器里登录微信

2. 在微信 PC 版或者 Mac 版，选择恢复消息到手机（参考 2.1）

3. 等待恢复完成。....

4. 在手机上登录微信

## 2.3 获取数据库
经过步骤 2.2，加密的聊天记录已经同步到模拟器（假手机）里了。我们需要把加密记录拿出来：

1. 从根（/）目录一路往下走 data, data, com.tencent.mm, MicroMsg, 一串数字字母（跟微信号相关的 MD5 摘要，可能会有两个，找有 EnMicroMsg.db 的那个）。

2. 选中加密的聊天记录数据库 EnMicroMsg.db（打上前面的对勾）

3. 点击 三，选择 内部存储设备，会快速跳到 /storage/emulated/0

4. 再选择共享文件夹（Mac 下默认是 $mumu共享文件夹）

5. 点击三个竖点

6. 点击 粘贴选择项，把加密的聊天记录数据库粘贴到共享文件夹

7. 点击 文件共享，就会弹出共享目录，电脑上便能拿到把加密的聊天记录数据库。

![image](/images/posts/2023-08-25/10.png)
获取加密数据库  
## 2.4 获取密钥
获取到了加密的数据库，这里就获取解密用的密钥。密钥由两部分组成：手机（这里是模拟器）的 IMEI 号（这是唯一的）和数据库对应微信号的 uin 码，再把这两个码拼起来，然后生成 MD5 摘要，前 7 位就是密钥了。

### 2.4.1 获取 IMEI 号
虽然说手机的 IMEI 号是唯一的，但模拟器的却不是。经验证，无论是 Mac 平台还是 Windows 平台，MuMu 模拟器的 IMEI 均为：1234567890ABCDEF。

### 2.4.2 获取 uin
1. 从根（/）目录一路往下走 data, data, com.tencent.mm, shared_prefs

2. 找到 auth_info_key_prefs.xml 文件，点击

3. 选择 使用编辑器打开，始终

4. 获取 uin，双引号里面的即为 uin（包括负号，如果有的话）

![image](/images/posts/2023-08-25/11.png)
获取 uin
### 2.4.3 生成密钥
把前面获取到的 IMEI 号和 uin 码（假设为：-123456789）放到下面的代码，即可获得密钥：*9eeab65*。

```
#! /usr/bin/env python3
# -*- coding: utf-8 -*-

import hashlib


def get_key(imei, uin):
    return hashlib.md5(f"{imei}{uin}".encode("utf-8")).digest().lower().hex()[:7]


if __name__ == "__main__":
    imei = "1234567890ABCDEF"
    uin = "-123456789"
    key = get_key(imei, uin)
    print(key)
```
不想写代码，也可以通过在线工具[3]来解决：把 IMEI 号和 uin 码拼在一起，得到：1234567890ABCDEF-123456789，输入在线工具，即可获得密钥：9eeab65（取前 7 位）。

## 2.5 解密
有了密钥，就可以进行解密了。

### 2.5.1 代码解密
写不动了，参考 1.3 自己改吧。

### 2.5.2 SQLCIPHER 解密
• Windows

下载 SQLCIPHER[4] 并解压，打开SQLCIPHER：

1. 点击文件夹图标，选择加密数据库

2. 输入密码

3. 点击确定

4. 切换到浏览数据功能

5. 选择消息表，然后就可以看到聊天记录了

6. 选择 File，Export，可以把聊天记录导出

![image](/images/posts/2023-08-25/12.png)
解密数据库  
• Mac

Mac 版下，用命令行会更方便些，打开 终端（Terminal），输入命令安装 sqlcipher：
```
brew install sqlcipher
```
什么？没有Homebrew？强烈建议使用，请参考：https://brew.sh/

安装好 sqlcipher 之后，就可以借助它来解密数据库了，把 {数据库} 替换成实际的数据库名（本例中为 EnMicroMsg.db），{密码} 替换成刚才获得的密钥：
```
sqlcipher {数据库} 'PRAGMA key = "{密码}"; PRAGMA cipher_page_size = 1024; PRAGMA cipher_compatibility = 1; ATTACH DATABASE "decrypted_database.db" AS decrypted_database KEY "";SELECT sqlcipher_export("decrypted_database");DETACH DATABASE decrypted_database;'

# 注意替换{数据库}和{密码}，如：
sqlcipher EnMicroMsg.db 'PRAGMA key = "9eeab65"; PRAGMA cipher_page_size = 1024; PRAGMA cipher_compatibility = 1; ATTACH DATABASE "decrypted_database.db" AS decrypted_database KEY "";SELECT sqlcipher_export("decrypted_database");DETACH DATABASE decrypted_database;'
```

解密完之后就得到了明文数据库 decrypted_database.db，然后可以使用各种工具打开进行下一步操作。当然，也可以用代码直接把聊天记录导出：
```
conn = sqlite3.connect("decrypted_database.db")
cur = conn.cursor()
sql = "select * from message"
cur.execute(sql)
res = cur.fetchall()
# 所有的聊天记录都在 res 里了（如果记录超多，可能会 OOM）
```
## 后话

## 引用链接
- [1] MuMu模拟器: https://mumu.163.com/
- [2] 《打开“共享文件夹”提示没有ROOT权限》: https://  mumu.163.com/help/20210513/35047_947573.html
- [3] 在线工具: http://emn178.github.io/online-tools/md5.html
- [4] SQLCIPHER: https://pan.baidu.com/s/1IhAipYUKNeu41N00jAIgzg?pwd=ya8z

