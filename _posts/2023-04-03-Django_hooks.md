---
layout:     post
title:      Django 序列化中的局部钩子和全局钩子
date:       2023-04-03
tag:        django
description: ""

---
> 参考：https://www.cnblogs.com/Richer-J/p/15247362.html

### 局部钩子
```csharp
         给某个字段再增加校验（固定用法）,定义一个方法，格式为 validate_字段名

```
```csharp
    from rest_framework.exceptions import ValidationError # 认证失败

    class UserModelSerializer(serializers.ModelSerializer)

        class Meta:
            model = User
            fields = '__all__'

            extra_kwargs = {    # 配置字段参数
                'username':{'read_only':True}
                'addr':{'max_length':16,'min_length':1,validators:[函数内存地址]}  # validators作用： 字段先校验自己的，最大长度最小长度，然后校验[]中的函数，它会触发这个函数，并且把字段传入函数里，这个函数校验不通过也会抛异常
            }

            # 限制addr，不能以dsb开头
            def validate_addr(self,a_data):

                if a_data.startswith('dsb')
                    # 抛异常，不能以dsb开头
                    raise ValidationError('不能以dsb开头')  # 以dsb开头没有报错，因为在APIView里捕获了全局异常，处理了，尽管抛异常，不会报错
                else:
                    return a_data

    # validators 与 validate_addr 的区别： validate_addr 只能用在addr字段上，而validators能用在addr、username等其他字段上
```

### 全局钩子
```csharp
    # 限制用户的名字不能等于用户的地址
    # 如果写局部钩子不能限制住，因为局部钩子只能拿一条数据即只能限制名字或者只能拿到地址，不能同时拿到名字和地址，所以需要全局钩子（固定写法）定义一个方法validate

    def validate(self, attrs):  # attrs 接收到所有字段数据
        name = attrs.get('username')
        addr = attrs.get('addr')
        if name == addr:
            raise ValidationError('name和addr不能一样')
        else:
            return attrs
```
