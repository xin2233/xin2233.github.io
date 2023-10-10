---
layout:     post
title:      django 跳转页面 html； render, redirect, HttpResponse 的区别
link:       [https://www.cnblogs.com/zjxcyr/p/15524682.html](https://www.cnblogs.com/zjxcyr/p/15524682.html)
date:       2021-11-08 16:02:00
tag:        django
description: ""
---


## 参考：

https://www.cnblogs.com/dylancao/p/11554906.html

https://blog.csdn.net/weixin_37989267/article/details/105764242

## 正文：

1.HttpResponse

它是作用是内部传入一个字符串参数，然后发给浏览器。

def index(request):  
return HttpResponse("index")  
2、render

render方法可接收三个参数，一是request参数，二是待渲染的html模板文件,三是保存具体数据的字典参数。

它的作用就是将数据填充进模板文件，最后把结果返回给浏览器。与jinja2类似。

def index(request):  
return render(request, "index.html", {"name": "pjb", })

3、redirect

接受一个URL参数，表示让浏览器跳转去指定的URL.

def index(request):  
return redirect("/index/")

## 实际例子：

### 第一种情况：

1.views.py 文件：

def index(request):

return render(request, 'index.html',locals())

2\. url.py 文件：

path('index/', views.index),

3\. index.html 文件

<a class="nav-link" href="/index/">Home</a>

### 第二种情况：

1.views.py 文件：

def index(request):

return render(request, 'index.html',locals())

2\. url.py 文件：
```csharp 
    path('login/', views.index, name='index'),  # 这里设置name，为了在模板文件中，写name，就能找到这个路由
```

3\. index.html 文件

<a class="nav-link" href="{% url 'index' %}">Home</a>

以上2中情况都可以实现网页跳转功能。
