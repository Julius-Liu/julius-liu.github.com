---
layout: post
title: Markdown 基本语法
categories: [blog]
tags : [Markdown, 前端开发]
description: Markdown 基本语法
---
快速跳转：<br>
[Markdown宗旨](#1) &ensp; [段落和换行](#2) &ensp; [标题](#3) &ensp; [区块引用](#4) <br/>
[列表](#5) &ensp; [代码区块](#6) &ensp; [分割线](#7) &ensp; [链接](#8) <br/>
[图片](#9) &ensp; [强调](#10) &ensp; [代码](#11) &ensp; [反斜杠转义](#12) <br/>

<h2 id="1">Markdown宗旨</h2>
Markdown 的目标是实现「易读易写」。<br/>
可读性，无论如何，都是最重要的。一份使用 Markdown 格式撰写的文件应该可以直接以纯文本发布，并且看起来不会像是由许多标签或是格式指令所构成。Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括Setext、atx、Textile、reStructuredText、Grutatext 和 EtText，而最大灵感来源其实是纯文本电子邮件的格式。<br/>
总之， Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然。比如：在文字两旁加上星号，看起来就像*强调*。Markdown 的列表看起来，嗯，就是列表。Markdown 的区块引用看起来就真的像是引用一段文字，就像你曾在电子邮件中见过的那样。<br/>
<br/>

<h2 id="2">段落和换行</h2>
一个 Markdown 段落是由一个或多个连续的文本行组成，它的前后要有一个以上的空行（空行的定义是显示上看起来像是空的，便会被视为空行。比方说，若某一行只包含空格和制表符，则该行也会被视为空行）。普通段落不该用空格或制表符来缩进。<br>
「由一个或多个连续的文本行组成」这句话其实暗示了 Markdown 允许段落内的强迫换行（插入换行符），这个特性和其他大部分的 text-to-HTML 格式不一样（包括 Movable Type 的「Convert Line Breaks」选项），其它的格式会把每个换行符都转成 `<br />` 标签。<br/>
为了中文段落的样式美观，通常会在段首使用首行缩进。实现首行缩进，可以采用以下两个方法：
1. 插入2个**全角**状态下的空格
1. 插入4个`&#160;`，这是HTML空格的ASCII码
<br/>

<h2 id="3">标题</h2>
标题应该是Markdown文件中最常用的一类标记。
推荐使用**类 Atx 形式**来创建标题，方法是是在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶。
另外，可以在末尾闭合行首的#，就像这样：

`## 这里是二级标题 ##`
<br/>

<h2 id="4">区块引用</h2>
Markdown 标记区块引用是使用类似 email 中用 > 的引用方式。如果你还熟悉在 email 信件中的引言部分，你就知道怎么在 Markdown 文件中建立一个区块引用，那会看起来像是你自己先断好行，然后在每行的最前面加上 > <br/>
你可以随意断行，在引用里还是会作为一个段落来处理
	
	> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
	  consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
	  Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
	> 
	> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
	  id sem consectetuer libero luctus adipiscing.

以上代码的显示效果，如下所示：

> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
  consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
  Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
  id sem consectetuer libero luctus adipiscing.
<br/>

<h2 id="5">列表</h2>
在行开始出添加星号或者短横，可以创建无序列表。<br>

    * 无序列表，项目1
    * 无序列表，项目2
    * 无序列表，项目3

* 无序列表，项目1
* 无序列表，项目2
* 无序列表，项目3

要创建有序列表，只要在行首加一个数字就可以，数字的序号可以任意，比如都写1：

    1. 有序列表，项目1
    1. 有序列表，项目2
    1. 有序列表，项目3

1. 有序列表，项目1
1. 有序列表，项目2
1. 有序列表，项目3
<br/>

<h2 id="6">代码区块</h2>
要手动创建代码区块，在写完一行以后，按下**Enter**，然后按一个**Tab**，Tab后的段落就会以它原本的形式出现。
下一行仍旧使用**Tab**开头，也会变成代码的第二行。<br/>
以下代码，截取自Timer.cpp

    int APIENTRY WinMain(HINSTANCE hInstance,
                     HINSTANCE hPrevInstance,
                     LPSTR     lpCmdLine,
                     int       nCmdShow)
	{
		HWND hwnd;
		
		if(hwnd=::FindWindow("ConsoleWindowClass",NULL)) {
			::ShowWindow(hwnd, SW_HIDE);
		}
		
		MSG msg;
		HACCEL hAccelTable;
	
		// initialize the gobal variables that holds
		// title and the window name
		strcpy(szTitle, "Timer Alert");
		strcpy(szWindowClass, "Window Class");
			
		// Initialize global strings
		MyRegisterClass(hInstance);
	
		// Perform application initialization:
		if (!InitInstance(hInstance, nCmdShow)) {
			return FALSE;
		}
	
		// Main message loop:
		while (GetMessage(&msg, NULL, 0, 0)) {
			if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg)) {
				TranslateMessage(&msg);
				DispatchMessage(&msg);
			}
		}

		return msg.wParam;
	}
<br/>

<h2 id="7">分割线</h2>
你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：<br/>

    ***
    ___

显示效果如下：

下面是一条分割线

***

上面是一条分割线
<br/>

<h2 id="8">链接</h2>
Markdown 支持两种形式的链接语法： 行内式和参考式两种形式。
不管是哪一种，链接文字都是用 **[方括号]** 来标记。
要建立一个**行内式**的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，有了**title**属性后，当鼠标移至链接标题，将会出现提示。<br/>

举例如下：

    This is [baidu](http://www.baidu.com "这里是百度的行内式链接！") inline link.
    This [baidu link](http://www.baidu.com) has no title attribute.

显示结果如下：

This is [baidu](http://www.baidu.com/ "这里是百度的行内式链接！") inline link.
This [baidu link](http://www.baidu.com/) has no title attribute.

**参考式**的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记：<br/>

    This is [baidu] [baidu_id] reference-style link.

接着，在文件的任意处，你可以把这个标记的链接内容定义出来：

    [baidu_id]: http://www.baidu.com  "这里是百度的参考式链接！"

显示结果如下：

This is [baidu] [baidu_id] reference-style link.
[baidu_id]: http://www.baidu.com  "这里是百度的参考式链接！"

**隐式**链接标记功能让你可以省略指定链接标记，这种情形下，链接标记会视为等同于链接文字，要用隐式链接标记只要在链接文字后面加上一个空的方括号，如果你要让 "baidu" 链接到 www.baidu.com，你可以简化成：<br/>

    [baidu][] <br/>

然后定义链接内容：

    [baidu]: http://www.baidu.com "这里是百度的隐式链接！"

显示结果如下所示：

[baidu][]

[baidu]: http://www.baidu.com "这里是百度的隐式链接！"

如果你觉得“baidu”这个变量名太长，也可以使用**标号**的方式：<br/>

    这里是 [百度][1] 的链接。
    这里是 [新浪][2] 的链接。
    [1]: http://www.baidu.com "这里是百度的隐式链接！"
    [2]: http://www.sina.com.cn "这里是新浪的隐式链接！"

显示效果如下：

这里是 [百度][1] 的链接。
这里是 [新浪][2] 的链接。

[1]: http://www.baidu.com "这里是百度的隐式链接！"
[2]: http://www.sina.com.cn "这里是新浪的隐式链接！"

当然如果你想提供一个最简单链接，可以使用**自动链接**的方式：

	<http://www.baidu.com>

显示效果如下：

<http://www.baidu.com>

> 优点：使用**参考式**链接或者**隐式**链接，可以让文件更像是浏览器最后产生的结果，可以增加链接而不让文章的阅读感觉被打断。
<br/>

<h2 id="9">图片</h2>
很明显地，要在纯文字应用中设计一个「自然」的语法来插入图片是有一定难度的。<br/>
Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式： **行内式**和**参考式**。<br/>
行内式的图片语法看起来像是：

    ![代替文字](/assets/images/cat.jpg)
    ![代替文字](/assets/images/cat.jpg "可选的title")

参考式的图片语法则长得像这样：

    ![代替文字][id]

[id] 是图片参考的名称，图片参考的定义方式则和连结参考一样：

    [id]: assets/images/cat.jpg  "可选的title"

给你看我最喜欢的猫咪，注意它的眼神。。

![cat image gone...][catImage]

[catImage]: /images/cat.JPG "为什么不给饭吃。。"
<br/>

<h2 id="10">强调</h2>
Markdown 使用星号（*）和底线（_）作为标记强调字词的符号，被 * 或 _ 包围的字词会被转成用 `<em>`(斜体) 标签包围，用两个 * 或 _包起来的话，则会被转成 `<strong>`(粗体)。<br/>

举例如下：

    *这里是斜体文字*
    **这里是粗体文字**

显示效果如下：<br/>

*这里是斜体文字* <br/>
**这里是粗体文字**
<br/>

<h2 id="11">代码</h2>
如果要标记一小段行内代码，你可以用反引号把它包起来（`），例如：

    Use the `printf()` function.

显示效果如下：<br/>

Use the `printf()` function.

这段语法会产生：

    <p><code>There is a literal backtick (`) here.</code></p>
<br/>

<h2 id="12">反斜杠转义</h2>
Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号，例如：如果你想要用星号加在文字旁边的方式来做出强调效果（但不用 <em> 标签），你可以在星号的前面加上反斜杠：

	\*literal asterisks\*

显示效果如下：

\*literal asterisks\*

Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

    \   反斜线
    `   反引号
    *   星号
    _   底线
    {}  花括号
    []  方括号
    ()  括弧
    #   井字号
    +   加号
    -   减号
    .   英文句点
    !   惊叹号

快速跳转：<br>
[Markdown宗旨](#1) &ensp; [段落和换行](#2) &ensp; [标题](#3) &ensp; [区块引用](#4) 
[列表](#5) &ensp; [代码区块](#6) &ensp; [分割线](#7) &ensp; [链接](#8) 
[图片](#9) &ensp; [强调](#10) &ensp; [代码](#11) &ensp; [反斜杠转义](#12)
<br/>

<div align="right">2016-02-10 星期三 8:59:20 PM</div>
