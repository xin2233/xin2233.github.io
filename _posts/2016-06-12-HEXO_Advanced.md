---
layout: post
title: HEXO进阶
tag: hexo
---

HEXO接近是最近有一些朋友提出的问题。

* 1、博客部署样式出问题了怎么办？
* 2、电脑重装或者误删了本地博客怎么办？
* 3、想使用两台电脑写博客怎么办？
* 4、为何使用百度搜不到我的博客？


### 使用Jekyll解决前三个问题。
不得不说 `Jekyll` 确实可以解决我上面三个问题, 因为 `Jekyll` 是直接把Markdown格式的文章直接放在github仓库里的, 相当于直接用git来管理博客了, `Github` 官方也很推荐 `Jekyll` 。 你可以先看下 `Jekyll` 搭建博客的[voyagelab](voyagelab.github.io), [github地址](https://github.com/voyagelab/voyagelab.github.io), 当然了这只是很普通的, Jekyll 也有很多主题可以选择的, 更详细的请看[Jekyll中文文档](http://jekyll.bootcss.com/)、[Jekyll英文文档](https://jekyllrb.com/)、[Jekyll主题列表](http://jekyllthemes.org/)。


### 使用Hexo解决上面前三个问题
是的, 我大`Hexo`同样可以解决上面三个问题, 那就是使用git。关于如何使用 `Hexo` 搭建博客请看我另一篇文章《HEXO搭建个人博客》, 如果搭建的过程中出现了问题, 我们可以交流交流。现在我假设你已经能基本使用 `Hexo` 了, 接下来就看看如何来管理博客。

## 使用git管理博客
结构大致是:

> -- Blog-Growing     
> 　　|-- .git     
> 　　|-- .gitignore    
> 　　|-- Hexo     
> 　　　　|   ..    
> 　　　　|   ..    
> 　　　　|   整个博客的配置信息    


### 具体实现:   
**一：家里电脑使用博客**        
　　建立git远端仓库管理博客,并使用家里的电脑把本地博客的配置推送到远端仓库。   
**二：公司电脑使用博客**         
　　到了公司只需要执行`sudo npm install -g hexo`,然后cd到你的博客目录下,如我cd 到Hexo目录下, 然后执行 `hexo server` 就可以在本地预览博客了。    
**三：使用Git保存**          
　　修改好博客后记得先使用git来提交下, 即使下次把博客的样式修改坏了, 也可以使用 `git reset --hard` 来回退。如: 我cd 到 `Blog-Growing` 目录下使用git提交。   
**四：博客提交**           
　　1、修改好的博客使用 `hexo d` 展示到博客页上。   
　　2、git push 整个本地博客。    

**提示:** 在这里 `git` 仅仅只是用户做博客的版本管理的, 博客的样式修改、基本部署还是使用 `hexo` 来操作的。

<br>

转载：[潘柏信的博客](http://leopardpan.cn) » [点击阅读原文](http://leopardpan.cn/2016/06/HEXO_Advanced/)