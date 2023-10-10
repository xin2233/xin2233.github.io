---
layout:     post
title:      Django forms表单 select下拉框的传值实例
link:       [https://www.cnblogs.com/zjxcyr/p/16773040.html](https://www.cnblogs.com/zjxcyr/p/16773040.html)
date:       2022-10-09 17:37:00
tag:        django
description: ""

---
forms.py
```csharp 
    class SignupForm(forms.Form):
      username = forms.CharField(validators=[user_unique_validate, username_rule_validate, ], required=True,
                    max_length=30, min_length=5,
                    error_messages={'required': '用户名不能为空', 'max_length': '用户名至少5位',
                            'min_length': '用户名最多30位'})
      password = forms.CharField(min_length=6, max_length=50, required=True,
                    error_messages={'required': '密码不能为空',
                            'invalid': '密码格式错误',
                            'min_length': '密码不能少于6位',
                            'max_length': '密码最多50位'})
      classInfo = forms.ModelChoiceField(queryset=ClassInfo.objects.all(), required=True, empty_label=None, initial="预设值")#这里加的是班级名字
      email = forms.EmailField(validators=[email_unique_validate, ], required=True,
                   error_messages={'required': '邮箱不能为空', 'invalid': '邮箱格式错误'})
      mobile = forms.CharField(validators=[mobile_validate, ], required=True,
                   error_messages={'required': '手机号不能为空'})
```

然后views通过get方法获得表单的form
```csharp 
    class SignupView(View):
      def get(self, request):
        obj = SignupForm()
        return render(request, 'user/signup.html', locals())
```

这里可以打印出来obj，可以看到表单类已经帮我们生成了前端代码
```csharp 
    <tr><th><label for="id_username">Username:</label></th><td><input type="text" name="username" value="123123" minlength="5" maxlength="30" required id="id_username"></td></tr>
    <tr><th><label for="id_password">Password:</label></th><td><input type="text" name="password" value="213123" minlength="6" maxlength="50" required id="id_password"></td></tr>
    <tr><th><label for="id_classInfo">Classinfo:</label></th><td>
    <select name="classInfo" id="id_classInfo">
     <option value="1" selected>15医药软件</option>
     <option value="2">15医药信息</option>
    </select></td></tr>
    <tr><th><label for="id_email">Email:</label></th><td><input type="email" name="email" value="123123231@qq.com" required id="id_email"></td></tr>
    <tr><th><label for="id_mobile">Mobile:</label></th><td><input type="text" name="mobile" value="13328768123" required id="id_mobile"></td></tr>
```

其中select下拉框里的内容是从数据库中取出来的，利用ModelChoiceField，设置queryset来取出数据，这样实现动态存取select中的值。

而前端代码可以直接使用这个表单变量obj
```csharp 
    <form method="post" action="{% url 'signup' %}">
      ...
      {% for field in obj %}
        {{ field }}
      {% end for %}
      ...
    </form>
```

但是我这里没有设置label值，就没有直接这样偷懒，而是自己写了一个
```csharp 
    <form method="post" action="{% url 'signup' %}>
    ....
       <tr>
         <td width="120" align="right" valign="top">用户名(学号)</td>
         <td width="auto" align="left"><input type="text" name="username" class="sls">        </td>
       </tr>
       <tr>
         <td width="120" align="right">密码</td>
         <td width="auto" align="left"><input type="password" name="password" class="sls"></td>
       </tr>
       <tr>
         <td width="120" align="right" valign="top">电子邮件</td>
         <td width="auto" align="left"><input type="text" name="email" class="sls"></td>
       </tr>
       <tr>
         <td width="120" align="right">班级</td>
         <td width="auto" align="left">
            {{ obj.classInfo }}
         </td>
       </tr>
       <tr>
         <td width="120" align="right" valign="top">手机号</td>
         <td width="auto" align="left"><input type="text" class="sls" name="mobile"></td>
       </tr>
    ....
```

前期我一直在用select标签来写，然后传值到option里，但是我发现通过再用obj.classInfo取里面的值时出现空白值

就类似于原本数据库存着两个选项，然后前端显示-------；选项一；空白；选项二；空白。

经过一番查找，出现-----这个选项是因为没有设置初始值，然后设置了initial

出现两个变量就是因为粗心大意，obj.classInfo本身就是个select标签，里面就有两个选项。

之后就是post提交验证，然后就是存值render操作了
```csharp 
      def post(self, request):
        has_error = True
        obj = SignupForm(request.POST)
        #print(obj)
        if obj.is_valid():
          has_error = False
          username = obj.cleaned_data['username']
          password = obj.cleaned_data['password']
          class_name = obj.cleaned_data['classInfo']
          email = obj.cleaned_data['email']
          mobile = obj.cleaned_data['mobile']
      ......
     
```

参考：https://www.jb51.net/article/165753.htm
