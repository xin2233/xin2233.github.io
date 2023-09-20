
### [xin2233的博客](http://xin2233.github.io)


### 环境要求

* Jekyll 支持: Mac 、Windows、ubuntu 、Linux 操作系统
* Jekyll 需要依赖: Ruby、bundler

### 使用手册

[Jekyll搭建个人博客](https://xin2233.github.io/2016/10/jekyll_tutorials1/)  :  使用Jekyll搭建个人博客的教程，及如何把这个博客模板修改成你自己的博客，里面也有大量的评论、Jekyll 搭建博客各种环境出现过的问题。

[HEXO搭建个人博客](https://xin2233.github.io/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/) : 使用 HEXO 基于 Github Page 搭建个人博客， 教程里面累计了大量提问和评论，如果你在搭建博客时遇到问题，可以看看这个教程。


#### 安装Jekyll

[Jekyll中文官方文档](http://jekyll.bootcss.com/) ， 如果你已经安装过了 Jekyll，可以忽略此处。

```
# 配置环境，参考ubuntu2004
sudo apt install -y ruby ruby-dev make gcc g++ zlib1g-dev

# 安装bundle
gem install bundle

# 进入项目路径下
cd /path/to/git/repo/

# 安装项目依赖
bundle install

# 進入目錄，开启本地服务
jekyll server
或者：
bundle exec jekyll server
```

在浏览器输入 [127.0.0.1:4000](127.0.0.1:4000) ， 就可以看到博客效果了。

### 头像效果

如果你只想要我博客里的头像效果，你只需要拿 xin2233.github.io/_includes/side-panel.html 文件里面 `头像效果` 和 xin2233.github.io/css/main.css 里面最后面 `头像效果` 部分就行了。

### 感謝

[Vno Jekyll](https://github.com/onevcat/vno-jekyll) 和 [leopardpan](https://github.com/leopardpan/leopardpan.github.io/)。


