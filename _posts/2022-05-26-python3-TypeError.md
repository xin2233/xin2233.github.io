---
layout:     post
title:      python3 TypeError: string argument expected, got 'bytes' 
link:       [https://www.cnblogs.com/zjxcyr/p/16314034.html](https://www.cnblogs.com/zjxcyr/p/16314034.html)
date:       2022-05-26 16:45:00
tag:        django
description: ""
---

参考：[(3条消息) python3 pycurl 出现 TypeError: string argument expected, got 'bytes' 解决方案_Konvin_Zhi的博客-CSDN博客](https://blog.csdn.net/yu599207582/article/details/59109815)

排查问题出现在使用StringIO的write方法上，用BytesIO替代StringIO即可解决问题；
```csharp 
        
```

from io import StringIO, BytesIO

# 内存文件操作
```csharp 
    _buf = BytesIO()  # 替换此处 BytesIO替代StringIO
        # 将图片保存在内存中，文件类型为png
        im.save(buf, 'png')_
```
