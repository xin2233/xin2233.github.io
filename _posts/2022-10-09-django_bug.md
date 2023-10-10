---
layout:     post
title:      解决Django中checkbox复选框的传值问题
link:       [https://www.cnblogs.com/zjxcyr/p/16773094.html](https://www.cnblogs.com/zjxcyr/p/16773094.html)
date:       2022-10-09 17:48:00
tag:        django
description: ""

---

# 解决Django中checkbox复选框的传值问题

Django 中，html 页面通过 form 标签来传递表单数据。

对于复选框信息，即 checkbox 类型，点击 submit 后，数据将提交至 view 中的函数。

我们通过request.POST.get() 函数来获取来自 html 页面的值，但是该函数只能 get 到选中的最后一个值。

因此想要传递选中的多个值，需要用 request.POST.getlist() 函数

该函数返回一个列表，可通过迭代来获取列表中每一项的值。
```csharp 
    HTML code
    <form action="" method="POST">
    
    <input type="checkbox" value="1" name="check_box_list"/>1
    
    <input type="checkbox" value="2" name="check_box_list"/>2
    
    <input type="checkbox" value="3" name="check_box_list"/>3
    
    <input type="submit" value="提交"/>
    </form>
```
```csharp 
    Python code
    def xxx(request):
    　　check_box_list = request.POST.getlist('check_box_list')
    　　#页面选中的checkbox的value都在这check_box_list里面 下面自己操作
```
