---
layout: post
title: "【已解决】smb失败：由软件导致的连接断开"
date: 2023-01-03
tags: ubuntu
description: "【已解决】smb失败：由软件导致的连接断开"
---

<h2 class="topic-title"><a href="https://forum.ubuntu.com.cn/viewtopic.php?f=116&amp;t=491129&amp;sid=72faa0830a4d5283f75e3a63f3f48dbb">【已解决】smb失败：由软件导致的连接断开</a></h2>
<p>英文版本如下：</p>
<p>Unable to access location. Failed to mount Windows share: Software caused connection abort</p>
<h2>问题：</h2>
<p><img src="/images/posts/2023-01-03/2620052-20221206112924223-379320470.png" alt="" width="450" height="385" loading="lazy" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>18.04升级到20.04就不行了</p>
<h2>解决方法</h2>
<p>编辑/etc/samba/smb.conf<br />在[global]后面添加<br />client min protocol = NT1<br />由于20.04把smb协议升级了，造成旧协议无法使用，我猜应该是重新启用smb1.0就行，如果是服务端就把client改称server</p>
