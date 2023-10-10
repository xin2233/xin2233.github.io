---
layout:     post
title:      在服务器上部署Django项目并使其在后台一直运行
link:       [https://www.cnblogs.com/zjxcyr/p/15806919.html](https://www.cnblogs.com/zjxcyr/p/15806919.html)
date:       2022-01-15 14:12:00
tag:        django
description: ""

---
1\. 在服务器中打开到 **manage.py __** 所在的目录，输入命令：
```csharp 
    python3 manage.py runserver 0.0.0.0:8000
    
```

2\. 后台运行

如果想要Django项目一直运行，关闭终端后还在运行，即需要运行如下命令，
```csharp 
    nohup python3 manage.py runserver 0.0.0.0:8000 &
    
```

Linux之后台执行命令：nohup和&的使用  
nohup 是 no hungup的缩写，以为“不挂断”,我们在使用Xshell等工具执行Linux脚本时，有时候会由于网络问题，导致失去连接，终端断开，程序运行一半就意外结束了。这种时候，就可以用nohup指令来运行指令，使程序可以忽略挂起信号继续运行。

**语法：**  
nohup Command [ Arg ... ] [ & ]  
nohup 命令运行由 Command参数和任何相关的 Arg参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用 nohup 命令运行后台中的程序。要运行后台中的 nohup 命令，添加 & （ 表示“and”的符号）到命令的尾部。

如果不将 nohup 命令的输出重定向，输出将附加到当前目录的 nohup.out 文件中。如果当前目录的 nohup.out 文件不可写，输出重定向到 $HOME/nohup.out 文件中。

**nohup和 &的区别**  
&：是指在后台运行，当用户退出（挂起）的时候，命令自动跟着结束  
nohup：不挂断的运行，注意并没有后台运行的功能，就是指用nohup运行命令可以使命令永久的执行下去，和用户终端没有关系，例如我们断开SSH连接都不会影响他的运行，注意了nohup没有后台运行的意思；&才是后台运行  
因此将nohup和&结合使用，就可以实现使命令永久地在后台执行的功能  
要实现守护进程，一种方法是按守护进程的规则去编程；另一种方法是仍然用普通方法编程，然后用nohup命令启动程序： 

nohup <程序名> &   
  


` `
