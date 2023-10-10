---
layout:     post
title:      "KeyErrorpasswordforms.py问题"
date:       2022-07-04
tag:        django
description: ""
---

## start
### 问题：

```
    Traceback (most recent call last):
      File "D:\doing\GIT\GitHub\Blog_Django\venv\lib\site-packages\django\core\handlers\exception.py", line 47, in inner
        response = get_response(request)
      File "D:\doing\GIT\GitHub\Blog_Django\venv\lib\site-packages\django\core\handlers\base.py", line 181, in _get_response
        response = wrapped_callback(request, *callback_args, **callback_kwargs)
      File "D:\doing\GIT\GitHub\Blog_Django\users\views.py", line 68, in register
        if form.is_valid():
      File "D:\doing\GIT\GitHub\Blog_Django\venv\lib\site-packages\django\forms\forms.py", line 175, in is_valid
        return self.is_bound and not self.errors
      File "D:\doing\GIT\GitHub\Blog_Django\venv\lib\site-packages\django\forms\forms.py", line 170, in errors
        self.full_clean()
      File "D:\doing\GIT\GitHub\Blog_Django\venv\lib\site-packages\django\forms\forms.py", line 372, in full_clean
        self._clean_fields()
      File "D:\doing\GIT\GitHub\Blog_Django\venv\lib\site-packages\django\forms\forms.py", line 393, in _clean_fields
        value = getattr(self, 'clean_%s' % name)()
      File "D:\doing\GIT\GitHub\Blog_Django\users\forms.py", line 46, in clean_password
        if self.cleaned_data['password'] != self.cleaned_data['password1']:
    KeyError: 'password1'

```

forms.py:

```
    class RegisterForm(forms.ModelForm):
        """注册表单"""
        email = forms.EmailField(label='邮箱', min_length=3, widget=forms.EmailInput(attrs={
            'class': 'form-contrl', 'placeholder': '邮箱'}))
        password1 = forms.CharField(label='密码', min_length=6, widget=forms.PasswordInput(attrs={
            'class': 'form-contrl', 'placeholder': '密码'}))
        password2 = forms.CharField(label='再次输入密码', min_length=6, widget=forms.PasswordInput(attrs={
            'class': 'form-contrl', 'placeholder': '再次输入密码'}))

        class Meta:
            model = User
            fields = ('email', 'password1')

        def clean_email(self):
            """ 验证用户是否存在 """
            email = self.cleaned_data.get('email')
            exists = User.objects.filter(email=email).exists()
            if exists:
                raise forms.ValidationError('邮箱已经存在!')
            return email

        def clean_password1(self):
            """验证密码是否相等"""
            print(self.cleaned_data)

            if self.cleaned_data['password1'] != self.cleaned_data['password2']:
                raise forms.ValidationError('两次密码输入不一致！')
            return self.cleaned_data['password1']
```

register.html:

```html
     <form action="" method="post">
                                csrf_token
                                {{ form.as_p }}
                                <input type="submit" style="margin-top: 1em;" class="button is-link is-fullwidth" value="注 册">
    </form>

```

### 原因分析：

因为在forms.py中字段是先定义 email，password1，password2，那么forms在clean data 的时候是按照e->p1->p2，那么在定义clean_<filename>()函数时，在clean_password1()中，password2还没有clean data，所以在forms.cleaned_data中就没有password2字段。

### 解决办法：

1.将forms.py中password1 和password2 的定义位置调换

2.不在clean_password1()函数中进行操作，既然是打算操作password1 和password2 那么就在clean_password2()函数中，进行操作。

### 参考：

> http://www.chenxm.cc/article/579.html
