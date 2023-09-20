---
layout: post
title: "如何禁用 Firefox 自动更新"
date: 2023-09-18
tags: 操作
description: ""
---

# 如何禁用 Firefox 自动更新 (macOS, Linux, Windows)

Posted by sysin on 2022-09-03

Estimated Reading Time 5 Minutes

Words 1.4k In Total

更新日期：Sat Sep 03 2022 20:10:00 GMT+0800，阅读量: 4688

请访问原文链接：[如何禁用 Firefox 自动更新 (macOS, Linux, Windows)](https://sysin.org/blog/disable-firefox-auto-update/ "如何禁用 Firefox 自动更新 (macOS, Linux, Windows)")，查看最新版。原创作品，转载请保留出处。

作者主页：[sysin.org](https://sysin.org/)

---

禁用浏览器自动更新系列文章：

- [如何禁用 Firefox 自动更新 (macOS, Linux, Windows)](https://sysin.org/blog/disable-firefox-auto-update/ "如何禁用 Firefox 自动更新 (macOS, Linux, Windows)")
- [如何禁用 Google Chrome 自动更新 (macOS, Linux, Windows)](https://sysin.org/blog/disable-chrome-auto-update/ "如何禁用 Google Chrome 自动更新 (macOS, Linux, Windows)")
- [如何禁用 Microsoft Edge 自动更新 (Windows, Linux, macOS)](https://sysin.org/blog/disable-edge-auto-update/ "如何禁用 Microsoft Edge 自动更新 (Windows, Linux, macOS)")

Firefox 官方提供了禁用自动更新的配置功能。Google Chrome 及基于其开源版本 Chromium 实现的衍生浏览器【国外各种（Microsoft Edge、Opera…），国产各种…】不仅没有禁用自动更新的配置，而且是强制自动更新如同病毒肆虐一般难以控制，非常不尊重用户。即使我们使用变通方法屏蔽了自动更新，它们竟然还会不停的提示软件已经过期。这也是笔者一直推崇并将 Firefox 作为主力浏览器的重要原因之一。

**适用的版本**：

本文写作时以 Firefox 80 版本为例，验证到 105 版本可用，不排除新版本将来可能有所变更。

如果方法失效，欢迎反馈，笔者将及时修正和更新，谢谢！

## Firefox for Linux

### 方法一：禁用 Firefox 自带更新功能

在 Linux 中，将配置文件放在 `Firefox/Distribution` 中，其中 `Firefox` 是 Firefox 的安装目录，不同的发行版会有所不同，您也可以通过将文件放在 `/etc/Firefox/policies` 目录下指定系统范围的策略。

在上述路径写入 policies.json 文件，内容如下：

```
{ 
    "policies": 
    { 
        "DisableAppUpdate": true 
    }
}
```



例如：在 Ubuntu 20.04 中，Firefox 默认安装在 `/usr/lib/firefox` 目录下，创建步骤如下：

```bash
## 默认 distribution 目录已经存在，若不存在手动创建
#mkdir /usr/lib/firefox/distribution
sudo sh -c "cat > /usr/lib/firefox/distribution/policies.json" << EOL
{
  "policies": {
    "DisableAppUpdate": true
  }
}
EOL
```



或者，直接在系统级别创建策略文件，无论 Firefox 安装路径如何：

```bash
sudo mkdir /etc/firefox/policies

sudo sh -c "cat > /etc/firefox/policies/policies.json" << EOL
{
  "policies": {
    "DisableAppUpdate": true
  }
}
EOL
```



### 方法二：禁用软件包管理中的更新

Linux 软件更新通常依赖于系统级别的包管理机制（例如 apt 和 yum），我们可以手动来控制是否更新。

> Firefox 稳定版（快速发行版）在 Linux 中的软件包名称为：firefox，另外有 Firefox（ESR）延长支持版（firefox-esr）

在 Debian 及衍生系统中禁用 Firefox 更新：

```bash
sudo apt-mark hold firefox
# 恢复
#sudo apt-mark unhold firefox

```



在 Redhat 及衍生系统中禁用 Firefox 更新：

```bash
echo 'exclude=firefox' >> /etc/yum.conf
# 恢复
#编辑 /etc/yum.conf 删除 exclude=firefox

```



## Firefox for Windows

### 方法一：使用策略文件

官方策略模板：https://github.com/mozilla/policy-templates/releases

创建策略文件 `<Firefox 安装目录>\distribution\policies.json`，内容如下：

```bash
{
  "policies": {
    "DisableAppUpdate": true
  }
}
```



### 方法二：使用注册表

操作步骤：

- 浏览到 “HKEY_LOCAL_MACHINE\Software\Policies” 创建项 “Mozilla”，在创建项 “Firefox”，创建完毕即 “HKEY_LOCAL_MACHINE\Software\Policies\Mozilla\Firefox”

- 在上述路径，右键点击空白处 (sysin)，新建一个 DWORD (32-Bit) Value，名称为 “DisableAppUpdate”

- 双击创建的 “DisableAppUpdate”，将值修改为 “1”。

直接使用注册表文件：

```bash
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Mozilla\Firefox]
"DisableAppUpdate"=dword:00000001
```



**直接使用 CMD**（推荐，最便捷）：

```bash
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Mozilla\Firefox" /v DisableAppUpdate /t REG_DWORD /d 1 /f

```



### 验证效果

此时 “选项” 中 “Firefox 更新”，提示 “更新已被系统管理员禁用”，检查更新按钮也不可用！

“关于 Firefox” 对话框中也可以看到提示 “更新已被系统管理员禁用”。

## 配置企业策略

下载[策略模板](https://github.com/mozilla/policy-templates)，支持 macOS 和 Windows。

- Mac 使用 plist 通过 MDM 部署
- Windows 中 admx 模板使用组策略部署

## Firefox 下载

> 备注：Firefox 区分界面语言，如果需要英文版将链接后缀 zh-CN 替换为 en-US 即可。

**Firefox 100 存档**：

- 百度网盘链接：https://pan.baidu.com/s/1s1RenThgQnTWALnKg0ytHQ 提取码：u5ic

**下载最新版（固定链接）**：

- macOS
  
  - [macOS (Universal) 简体中文版](https://download.mozilla.org/?product=firefox-latest-ssl&os=osx&lang=zh-CN)

- Linux
  
  - [Linux 64-bit 简体中文版](https://download.mozilla.org/?product=firefox-latest-ssl&os=linux64&lang=zh-CN)
  - [Linux 32-bit 简体中文版](https://download.mozilla.org/?product=firefox-latest-ssl&os=linux&lang=zh-CN)

- Windows
  
  - [Windows 64-bit 简体中文版](https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=zh-CN)
  
  - [Windows 64-bit MSI 简体中文版](https://download.mozilla.org/?product=firefox-msi-latest-ssl&os=win64&lang=zh-CN)
  
  - [Windows ARM64/AArch64 简体中文版](https://download.mozilla.org/?product=firefox-latest-ssl&os=win64-aarch64&lang=zh-CN)
  
  - [Windows 32-bit 简体中文版](https://download.mozilla.org/?product=firefox-latest-ssl&os=win&lang=zh-CN)
  
  - [Windows 32-bit MSI 简体中文版](https://download.mozilla.org/?product=firefox-msi-latest-ssl&os=win&lang=zh-CN)

**如何下载指定版本**：

- 将下载链接中的 latest 替换为版本号，例如：90.0

- 或者访问 [Firefox Release Download](https://ftp.mozilla.org/pub/firefox/releases/)](https://ftp.mozilla.org/pub/firefox/releases/)

## 其他

上面文章内容我只试验了firefox win版本，ok

仅备份内容，其他文章请看原文。
