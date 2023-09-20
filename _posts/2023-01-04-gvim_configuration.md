---
layout: post
title: "gvim 配置"
date: 2023-01-04
tags: linux
description: "gvim 配置"
---

<p>参考：</p>
<p><a href="https://www.cnblogs.com/ryzz/p/12554617.html">gVim设置默认主题和字体（图文详细版） - RYZZ - 博客园 (cnblogs.com)</a></p>
<p>&nbsp;</p>
<blockquote>
<p>软件下载：https://pc.qq.com/detail/1/detail_3021.html&nbsp;</p>
</blockquote>
<h2>建议直接复制下面文章中的配置文件：</h2>
<p id="articleContentId" class="title-article">gvim的字符集，编码等utf-8设置</p>
<div class="cnblogs_code">
<pre><span style="color: #008080;"> 1</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 设置行号</span>
<span style="color: #008080;"> 2</span> <span style="color: #000000;">set number
</span><span style="color: #008080;"> 3</span> 
<span style="color: #008080;"> 4</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 编码设置</span>
<span style="color: #008080;"> 5</span> set enc=utf-<span style="color: #800080;">8</span>
<span style="color: #008080;"> 6</span> <span style="color: #800000;">"</span><span style="color: #800000;"> set fencs=utf-8</span>
<span style="color: #008080;"> 7</span> <span style="color: #800000;">"</span><span style="color: #800000;"> ,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936</span>
<span style="color: #008080;"> 8</span>  
<span style="color: #008080;"> 9</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 设置菜单语言</span>
<span style="color: #008080;">10</span> set langmenu=zh_CN.UTF-<span style="color: #800080;">8</span>
<span style="color: #008080;">11</span>  
<span style="color: #008080;">12</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 导入删除菜单脚本，删除乱码的菜单</span>
<span style="color: #008080;">13</span> source $VIMRUNTIME/<span style="color: #000000;">delmenu.vim
</span><span style="color: #008080;">14</span>  
<span style="color: #008080;">15</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 导入正常的菜单脚本</span>
<span style="color: #008080;">16</span> source $VIMRUNTIME/<span style="color: #000000;">menu.vim
</span><span style="color: #008080;">17</span>  
<span style="color: #008080;">18</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 设置提示信息语言</span>
<span style="color: #008080;">19</span> language messages zh_CN.utf-<span style="color: #800080;">8</span>
<span style="color: #008080;">20</span>  
<span style="color: #008080;">21</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 字体设置</span>
<span style="color: #008080;">22</span> set guifont=<span style="color: #000000;">Consolas:h10:cANSI:qDRAFT
</span><span style="color: #008080;">23</span> 
<span style="color: #008080;">24</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 命令行（在状态行下）的高度，默认为1，这里是2</span>
<span style="color: #008080;">25</span> <span style="color: #800000;">"</span><span style="color: #800000;"> set cmdheight=2</span>
<span style="color: #008080;">26</span> 
<span style="color: #008080;">27</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 设置搜索大小写不敏感</span>
<span style="color: #008080;">28</span> <span style="color: #000000;">set ignorecase
</span><span style="color: #008080;">29</span> 
<span style="color: #008080;">30</span> <span style="color: #800000;">"</span><span style="color: #800000;"> hi CursorLine term=bold cterm=bold ctermbg=Red</span>
<span style="color: #008080;">31</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 当前行不仅被高亮成了红色，而且还变成了粗体，这就是命令中bold和Red的效果，其中cterm=bold就是指定在终端中被高亮的行变为粗体，而 ctermbg=Red就是指定高亮行在终端中的背景色，其他的选项还有ctermfg(前景色)，guibg(gvim中的背景色)等等，这里就不赘述了。</span>
<span style="color: #008080;">32</span> <span style="color: #800000;">"</span><span style="color: #800000;"> 设置当前行高亮</span>
<span style="color: #008080;">33</span> <span style="color: #000000;">set cursorline 
</span><span style="color: #008080;">34</span> <span style="color: #800000;">"</span><span style="color: #800000;"> cterm：【bold粗体】，guibg【gvim中的背景色】，ctermbg=Red【指定高亮行在终端中的背景色】，ctermfg【前景色】，</span>
<span style="color: #008080;">35</span> hi CursorLine   cterm=NONE ctermbg=darkred ctermfg=<span style="color: #000000;">white 
</span><span style="color: #008080;">36</span> hi CursorColumn cterm=NONE ctermbg=darkred ctermfg=white </pre>
</div>
<p>&nbsp;</p>
<p><span style="background-color: #ffff99;">有更多想更改的，参考下面连接：</span></p>
<p><a href="https://blog.csdn.net/u010074478/article/details/35987479">(14条消息) gvim的字符集，编码等utf-8设置_爱菜鸟高高飞的博客-CSDN博客_gvim utf8</a></p>
<p>&nbsp;</p>
<h2>问题1：解决方法。</h2>
<p><a href="https://blog.csdn.net/guyue35/article/details/103426391">(14条消息) gvim菜单栏乱码解决办法_guyue35的博客-CSDN博客_gvim 乱码</a></p>
<p>&nbsp;</p>
<p>enc：</p>
<p>1. encoding<br />&nbsp;&nbsp;&nbsp; 表示vim自身内部使用的编码方式，如内部缓冲，菜单，消息等的编码方式。如果你现在正以不同于encoding的编码编辑一个文件，如使用set，它并不决定文件被保存的编码方式，也不能指导vim我们要打开的文件是什么格式。它不是作这个用的。</p>
<p>&nbsp;</p>
<p>2. fileencoding<br />&nbsp;&nbsp;&nbsp; 表示VIM所认为的当前被处理的文件的编码格式，即告诉我们当前打开的文件的格式。而且如果我们保存文件的话，vim也会以此格式保存，就算这个fileencoding并不是真正的fileencoding。举个例子，有个文件是utf-8编码，而且我们的vim将他成功打开了，即vim识别出他是utf-8编码，这样我们在vim里面执行:set fileencoding命令得到的结果就是fileencoding=utf-8，可见vim已经识别出了这个文件的格式，而且以此格式对文件操作，如果我们手动修改fileencoding即：执行:set fileencoding=cp936的话，然后保存文件，得到的文件就将自动被转换成了cp936格式，可以用file filename命令来查看该文件的格式，的确如此。如果还想转换回来，就可以像刚才那样再次用vim打开，然后:set fileencoding=utf-8然后保存，当然也可以用iconv命令即：<br />iconv -f cp936 -t utf-8 -o targetfile sourcefile 即将按照cp936格式编码的文件sourcefile转换成按照utf-8编码的文件targetfile. 此外，不管你怎么变换，我们一直没有改动encoding，而且使用:set encoding命令查看其值，也是一直没有变，它的值如果我们不人工指定的话，一般来说默认等于我们的LANG里面的设置的字符集，我们目前默认的LANG=en_US.UTF-8，encoding一般会与LANG的locale设置保持一致的。可见，不管你认为文件是什么格式，我的encoding一直不变，那么我再进行文件处理的时候，就会在内部用一种格式，即encoding指定的格式，当然首先要从fileencoding指定的格式转换成encoding的格式，处理后，在转换成fileencoding的格式，具体的转换依赖的是iconv的功能。<br />&nbsp; &nbsp; 那么，fileencoding的格式我们总不能每打开一个文件手动指定吧，vim时可以自动检查的，当检查成功后，就会自动设置fileencoding的值，这就是fileencodings的功能。&nbsp;</p>
<p>&nbsp;</p>
<p>3. fileencodings<br />&nbsp;&nbsp;&nbsp; 这是一个列表，他一般包含多个值，VIM在打开文件的时候会从这个列表中依次拿出一个值与被打开的文件比较，直到找到匹配的编码方式a。然后fileencoding就会被设置成a。这样当你对文件编辑的时候就会使用a.<br /><br />下面是一段网上的介绍的摘抄：<br />vim里面的编码主要跟三个参数有关：enc(encoding), fenc(fileencoding)和fencs(fileencodings)<br />其中fenc是当前文档的编码，也就是说，一个在vim里面已正确显示了的文档(前提是您的系统环境跟您的enc配置匹配)，您能够通过改变fenc后再w来将此文档存成不同的编码。比如说，我:set fenc=utf-8然后:w就把文档存成utf-8的了，:setfenc=gb18030再:w就把文档存成gb18030的了。这个值对于打开文档的时候是否能够正确地解码没有任何关系。<br />fencs就是用来在打开文档的时候进行解码的猜测列表。文档编码没有百分百正确的判断方法，所以vim只能猜测文档编码。比如我的vimrc里面这个的配置是<br />set fileencodings=utf-8,gb18030,utf-16,big5<br />所以我的vim每打开一个文档，先尝试用utf-8进行解码，假如用utf-8解码到了一半出错(所谓出错的意思是某个地方无法用utf-8正确地解码)，那么就从头来用gb18030重新尝试解码，假如gb18030又出错(注意gb18030并不是像utf-8似的规则编码，所以所谓的出错只是说某个编码没有对应的有意义的字，比如0)，就尝试用utf-16，仍然出错就尝试用big5。这一趟下来，假如中间的某次解码从头到尾都没有出错，那么vim就认为这个文档是这个编码的，不会再进行后面的尝试了。这个时候，fenc的值就会被设为vim最后采用的编码值，能够用:setfenc?来查看具体是什么。<br />当然这个也是有可能出错的，比如您的文档是gb18030编码的，但是实际上只有一两个字符是中文，那么有可能他们正好也能被utf-8解码，那么这个文档就会被误认为是utf-8的导致错误解码。<br />至于enc，其作用基本只是显示。不管最后的文档是什么编码的，vim都会将其转换为当前系统编码来进行处理，这样才能在当前系统里面正确地显示出来，因此enc就是干这个的。在windows下面，enc默认是cp936，这也就是中文windows的默认编码，所以enc是无需改的。在linux下，随着您的系统locale可能设为zh_CN.gb18030或zh_CN.utf-8，您的enc要对应的设为gb18030或utf-8(或gbk之类的)。<br />最后再来说一下新建空文档的默认编码。看文档似乎说会采用fencs里面的第一个编码作为新建文档的默认编码。但是这里有一个问题，就是fencs的顺序跟解码成功率有很大关系，根据我的经验utf-8在前比gb18030在前成功率要高一些，那么假如我新建文档默认想让他是gb18030编码怎么办？一个方法是每次新建文档后都:set fenc=gb18030一下，但是我发现在vimrc里面配置fenc=gb18030也能达到这个效果。<br />总结一下，我的vimrc里面的配置是：<br />set fileencoding=gb18030<br />set fileencodings=utf-8,gb18030,utf-16,big5</p>
<p>另外enc根据环境来设。</p>