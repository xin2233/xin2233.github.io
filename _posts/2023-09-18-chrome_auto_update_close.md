---
layout: post
title: "如何禁用 Google Chrome 自动更新"
date: 2023-09-18
tags: 操作
description: ""
---

# 如何禁用 Google Chrome 自动更新 (macOS, Linux, Windows)

Posted by sysin on 2022-09-03

Estimated Reading Time 11 Minutes

Words 2.5k In Total

更新日期：Sat Sep 03 2022 20:20:00 GMT+0800，阅读量: 10851

请访问原文链接：[如何禁用 Google Chrome 自动更新 (macOS, Linux, Windows)](https://sysin.org/blog/disable-chrome-auto-update/ "如何禁用 Google Chrome 自动更新 (macOS, Linux, Windows)")，查看最新版。原创作品，转载请保留出处。

作者主页：[sysin.org](https://sysin.org/)

---

禁用浏览器自动更新系列文章：

- [如何禁用 Firefox 自动更新 (macOS, Linux, Windows)](https://sysin.org/blog/disable-firefox-auto-update/ "如何禁用 Firefox 自动更新 (macOS, Linux, Windows)")
- [如何禁用 Google Chrome 自动更新 (macOS, Linux, Windows)](https://sysin.org/blog/disable-chrome-auto-update/ "如何禁用 Google Chrome 自动更新 (macOS, Linux, Windows)")
- [如何禁用 Microsoft Edge 自动更新 (Windows, Linux, macOS)](https://sysin.org/blog/disable-edge-auto-update/ "如何禁用 Microsoft Edge 自动更新 (Windows, Linux, macOS)")

不同于 Firefox 官方提供了禁用自动更新的配置功能，Google Chrome 及基于其开源版本 Chromium 实现的衍生浏览器【国外各种（Microsoft Edge 等），国产各种…】不仅没有禁用自动更新的配置，而且是强制自动更新如同病毒病毒肆虐一般难以控制，非常不尊重用户。即使我们使用变通方法屏蔽了自动更新，它们竟然还会不停的提示软件已经过期。

**适用的版本**：

本文写作时以 Chrome 88 版本为例，验证到 105 版本可用，不排除新版本将来可能有所变更。

如果方法失效，欢迎反馈，笔者将及时修正和更新，谢谢！

## Google Chrome for Linux

官方发行的 deb 和 rpm 包是不带更新功能的，通过第三方 repo 安装的 Chrome 通常会通过软件包管理进行更新。

### 通过软件包管理禁用自动更新

Linux 软件更新通常依赖于系统级别的包管理机制（例如 apt 和 yum），我们可以手动来控制是否更新。

> Chrome 稳定版在 Linux 中的软件包名称为：google-chrome-stable

在 Debian 及衍生系统中禁用 Chrome 更新：

```bash
sudo apt-mark hold google-chrome-stable
# 恢复
#sudo apt-mark unhold google-chrome-stable

```



在 Redhat 及衍生系统中禁用 Chrome 更新：

```bash
echo 'exclude=google-chrome-stable' >> /etc/yum.conf
# 恢复
#编辑 /etc/yum.conf 删除 exclude=google-chrome-stable

```



### 通过策略禁用更新

以下直接引用官方文档，有兴趣可以自行测试。

Step 1: Turn off Chrome browser updates

To stop Chrome browser auto-updating, take one of the following actions:

- Create an empty repository before installing Chrome browser:  
  `$ sudo touch /etc/default/google-chrome`
- Add the following line to **/etc/default/google-chrome**:  
  `repo_add_once=false`

Step 2: Turn off Chrome browser component updates (Optional)

*Applies only to Chrome browser components.*

Even if you turn off automatic updates for Chrome browser, browser components won’t automatically stop updating, including Widevine DRM (for encrypted media) and the Chrome updater recovery component. If you want to stop these components from updating, disable the Chrome [ComponentUpdatesEnabled](https://cloud.google.com/docs/chrome-enterprise/policies/?policy=ComponentUpdatesEnabled) policy.

Using your preferred JSON file editor:

1. In your **etc/opt/chrome/policies/managed** folder, create a JSON file and name it **component_update.json**.

2. Add the following setting to the JSON file to turn off component updates:
   
   ```bash
   {
   "ComponentUpdatesEnabled": "false"
   }
   
   ```
   
   

3. Deploy the update to your users.

## Google Chrome for Windows

### Chrome for Windows 如何自动更新？

本文写作时以 Chrome 88 版本为例，100 版本测试任务计划名称有所变化，现在已经验证到 105 版本可用，不排除新版本将来可能有所变更。

Chrome 在 Windows 平台同时发布三个个版本，分别是：

- 系统版（为所有用户安装）即 Windows System Setup，安装在 `Program Files` 文件夹下，需要管理员权限安装；
- 企业版与上述系统版安装路径和权限要求是一样的，区别在于不是 exe 文件而是 msi 格式的安装包，msi 格式可以用于组策略部署；
- 用户版即 Windows User Setup，安装在 `Users` 文件夹下，不需要管理员权限，普通用户就可以安装。

**用户版不带自动更新程序，解压后是一个 7z 文件，即绿色版。**

```bash
用户版主程序安装路径：
C:\Users\<用户名>\AppData\Local\Google\Chrome\Application\chrome.exe

```



**系统版或者企业版使用以下方法进行自动更新**：

```bash
更新服务：
Google 更新服务 (gupdate)
Google 更新服务 (gupdatem)
Google Chrome Elevation Service (GoogleChromeElevationService)

任务计划：
GoogleUpdateTaskMachineCore
GoogleUpdateTaskMachineUA

新版的任务计划名称有所变化，原有名称都加上了随机字符串，例如：
GoogleUpdateTaskMachineCore{8A32A951-669F-4A7C-A7D0-F4169A5E59F0}
GoogleUpdateTaskMachineCore{8A32A951-669F-4A7C-A7D0-F4169A5E59F0}

主程序安装路径：
x64
C:\Program Files\Google\Chrome\Application\chrome.exe
x86
C:\Program Files (x86)\Google\Chrome\Application\chrome.exe

更新程序 GoogleUpdate.exe 路径：
x64 和 x86 版本相同
C:\Program Files (x86)\Google\Update\GoogleUpdate.exe

```



**根据上述路径，手动禁用或者删除即可禁用自动更新**，即分别禁用或删除以下：

- 更新服务
- 任务计划
- 删除更新程序（整个 Update 文件夹）

### 使用 PowerShell 禁用更新

打开 Windows PowerShell 直接复制以下脚本运行一下更加方便：

> 或者将脚本保存为 `disable-chrome-auto-update.ps1` 文件，右键点击 “使用 PowerShell 运行” 即可快速完成。

```bash
if ([Environment]::Is64BitOperatingSystem -eq "True") {
    #Write-Host "64-bit OS"
    $PF=${env:ProgramFiles(x86)}
}
else {
    #Write-Host "32-bit OS"
    $PF=$env:ProgramFiles
}

if ($(Test-Path "$env:ProgramFiles\Google\Chrome\Application\chrome.exe") -eq "true") {
    # 结束进程
    taskkill /im chrome.exe /f
    taskkill /im GoogleUpdate.exe /f
    # Google Chrome 更新服务 (sysin)
    #这里也可以使用 sc.exe stop "service name"
    Stop-Service -Name "gupdate"
    Stop-Service -Name "gupdatem"
    Stop-Service -Name "GoogleChromeElevationService"
    # Windows 10 默认 PS 版本 5.1 没有 Remove-Service 命令
    # This cmdlet was added in PS v6. See https://docs.microsoft.com/en-us/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6#cmdlet-updates.
    #Remove-Service -Name "gupdate"
    #Remove-Service -Name "gupdatem"
    #Remove-Service -Name "GoogleChromeElevationService"
    # sc 在 PowerShell 中是 Set-Content 别名，所以要使用 sc.exe 否则执行后无任何效果
    sc.exe delete "gupdate"
    sc.exe delete "gupdatem"
    sc.exe delete "GoogleChromeElevationService"
    # 任务计划企业版
    #schtasks.exe /Delete /TN \GoogleUpdateBrowserReplacementTask /F
    #schtasks.exe /Delete /TN \GoogleUpdateTaskMachineCore /F
    #schtasks.exe /Delete /TN \GoogleUpdateTaskMachineUA /F
    Get-ScheduledTask -taskname GoogleUpdate* | Unregister-ScheduledTask -Confirm: $false
    # 移除更新程序
    Remove-Item "$PF\Google\Update\" -Recurse -Force
    Write-Output "Disable Google Chrome Enterprise x64 Auto Update Successful!"
}
elseif ($(Test-Path "${env:ProgramFiles(x86)}\Google\Chrome\Application\chrome.exe") -eq "true") {
    # 结束进程
    taskkill /im chrome.exe /f
    taskkill /im GoogleUpdate.exe /f
    # 删除 Google Chrome 更新服务
    #这里也可以使用 sc.exe stop "service name"
    Stop-Service -Name "gupdate"
    Stop-Service -Name "gupdatem"
    Stop-Service -Name "GoogleChromeElevationService"
    # Windows 10 默认 PS 版本 5.1 没有 Remove-Service 命令，糟糕！
    # This cmdlet was added in PS v6. See https://docs.microsoft.com/en-us/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6#cmdlet-updates.
    #Remove-Service -Name "gupdate"
    #Remove-Service -Name "gupdatem"
    #Remove-Service -Name "GoogleChromeElevationService"
    # sc 在 PowerShell 中是 Set-Content 别名，所以要使用 sc.exe 否则执行后无任何效果
    sc.exe delete "gupdate"
    sc.exe delete "gupdatem"
    sc.exe delete "GoogleChromeElevationService"
    # 删除任务计划
    #schtasks.exe /Delete /TN \GoogleUpdateBrowserReplacementTask /F
    #schtasks.exe /Delete /TN \GoogleUpdateTaskMachineCore /F
    #schtasks.exe /Delete /TN \GoogleUpdateTaskMachineUA /F
    Get-ScheduledTask -taskname GoogleUpdate* | Unregister-ScheduledTask -Confirm: $false
    # 移除更新程序
    Remove-Item "$PF\Google\Update\" -Recurse -Force
    Write-Output "Disable Google Chrome Enterprise x86 Auto Update Successful!"
}
else {
    Write-Output "No Google Chrome Enterprise Installation Detected!"
}
```



### 组策略配置更新（仅适用于域客户端）

下载 [administrative template](https://support.google.com/chrome/a/answer/6350036)，通过组策略部署，该方式适合企业域管理员，不再赘述。

## 下载 Chrome

**Chrome 100 存档**：

- 百度网盘链接：[百度网盘 请输入提取码](https://pan.baidu.com/s/1gdz5Yau7d9Z-jPOI-EIfdg) 提取码：7nlf

**Chrome 105 存档**：

- 百度网盘链接：[百度网盘 请输入提取码](https://pan.baidu.com/s/1r62QudAEW-KD3-nSBpeoXw) 提取码：ljlo

[Google Chrome 策略配置](https://support.google.com/chrome/a/answer/9049675)

Google Chrome 下载

备注：Chrome 内置多国语言界面。

### Chrome macOS 最新稳定版固定下载地址

[Chrome for macOS for Intel chip](https://dl.google.com/chrome/mac/stable/GGRO/googlechrome.dmg) (已停止更新)

[Chrome for macOS for Apple chip & Intel chip](https://dl.google.com/chrome/mac/universal/stable/GGRO/googlechrome.dmg)

### Chrome Linux 最新稳定版固定下载地址

[Chrome for Linux – deb](https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb)

[Chrome for Linux – rpm](https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm)

### Chrome Windows 最新版（3 种）下载地址

- Windows System Setup 最新稳定版固定下载地址
  
  (安装在 Program Files 文件夹下，需要管理员权限安装)
  
  [Google Chrome System Setup x86 - 32bit](https://dl.google.com/tag/s/appguid%3D%7B8A69D345-D564-463C-AFF1-A69D9E530F96%7D%26iid%3D%7BC8677E28-5C52-39F1-E4F4-28C874ED170E%7D%26lang%3Den%26browser%3D4%26usagestats%3D0%26appname%3DGoogle%2520Chrome%26needsadmin%3Dtrue/update2/installers/ChromeStandaloneSetup.exe)
  
  [Google Chrome System Setup x64 - 64bit](https://dl.google.com/tag/s/appguid%3D%7B8A69D345-D564-463C-AFF1-A69D9E530F96%7D%26iid%3D%7BC8677E28-5C52-39F1-E4F4-28C874ED170E%7D%26lang%3Den%26browser%3D4%26usagestats%3D0%26appname%3DGoogle%2520Chrome%26needsadmin%3Dtrue%26ap%3Dx64-stable/update2/installers/ChromeStandaloneSetup64.exe)

- Windows User Setup (安装在 Users 文件夹下)
  
  亦称为 Google Chrome for single user account，需要文明访问。
  
  可以搜索第三方网站查看无需文明访问的链接 (sysin)。
  
  [例如](http://viewver.coolpage.biz/chrome.php)：
  
  - 64位：https://redirector.gvt1.com/edgedl/release2/chrome/ad7mhuhjnyvwrrzylewtotyzpdqa_100.0.4896.127/100.0.4896.127_chrome_installer.exe
  - 32位：https://redirector.gvt1.com/edgedl/release2/chrome/koeiup3mgqmcikqtwgqsyeldma_100.0.4896.127/100.0.4896.127_chrome_installer.exe
  
  **用户版不带自动更新程序。解压即为绿色版。**

- Windows MSI 安装包，企业版
  
  - [googlechromestandaloneenterprise64.msi - 64bit](https://dl.google.com/tag/s/dl/chrome/install/googlechromestandaloneenterprise64.msi)
  - [googlechromestandaloneenterprise.msi - 32bit](https://dl.google.com/tag/s/dl/chrome/install/googlechromestandaloneenterprise.msi)



----

## 其他

上面内容仅尝试了chrome win 版本，ok。

仅做内容备份目的。其他文章请看原文。



---


