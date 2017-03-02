---
title: HTML 基础
date: 2016-09-14 
---

## HTML标题是通过< h1 > - < h6 >标签来实现的。例如：

* < h1>我是标题111</h1>
* < h2>我是标题222</h2>
* < h3>我是标题333</h3>

h1 - h6 的区别在于显示出的标题字体的大小不同。应该将 h1 用作主标题（最重要的），其后是 h2（次重要的），再其次是 h3，以此类推。

## HTML 水平线

< hr / > 标签在 HTML 页面中创建水平线。

< p>This is a paragraph</p>
< hr />
< p>This is a paragraph</p>
< hr />
< p>This is a paragraph</p>

## HTML 段落

HTML 段落是通过< p >来进行实现的。如：
< p>我是段落一</p>
< p>我是段落二</p>

## HTML 折行

如果您希望在不产生一个新段落的情况下进行换行（新行），请使用 < br /> 标签：

< p>This is<br />a para<br />graph with line breaks</p>

## HTML 链接

HTML 链接是通过< a > 来进行定义的。如：

< a href="http://www.w3school.com.cn"> This is a link</a>
在 href 属性中指定链接的地址。

## HTML 图像

HTML 图像是通过< img > 来进行定义。 如：

< img src="w3school.jpg" width="104" height="142" />
图像的名称和尺寸是以属性的形式提供的。

## HTML 注释

可以将注释插入 HTML 代码中，这样可以提高其可读性，使代码更易被人理解。浏览器会忽略注释，也不会显示它们。注释是这样写的：

< !-- This is a comment -->

## HTML 元素

HTML文档是通过HTML元素定义的。HTML元素是指从开始标签到结束标签之间的所有代码。

## HTML 元素语法：

HTML 元素以开始标签起始
HTML 元素以结束标签终止
元素的内容是开始标签与结束标签之间的内容
某些 HTML 元素具有空内容（empty content）
空元素在开始标签中进行关闭（以开始标签的结束而结束）
大多数 HTML 元素可拥有属性
大多数 HTML 元素可以嵌套。HTML 文档由嵌套的 HTML 元素构成。举个栗子：

``` objc
< html>
< body>
< p>This is my first paragraph.</p>
< /body>
< /html>
``` 

以上代码包含了三个HTML元素。
其中，元素定义了HTML文档中的一个段落。元素是HTML文档的主体部分。元素定义了整个HTML文档。

## 空的HTML元素

没有内容的 HTML 元素被称为空元素。空元素是在开始标签中关闭的。
如：
就是没有关闭标签的空元素（标签定义换行）。

在 XHTML、XML 以及未来版本的 HTML 中，所有元素都必须被关闭。

在开始标签中添加斜杠，比如 
，是关闭空元素的正确方法，HTML、XHTML 和 XML 都接受这种方式。

即使 在所有浏览器中都是有效的，但使用 
其实是更长远的保障。

HTML 提示：使用小写标签
HTML 标签对大小写不敏感：

等同于

。许多网站都使用大写的 HTML 标签。
W3School 使用的是小写标签，因为万维网联盟（W3C）在 HTML 4 中推荐使用小写，而在未来 (X)HTML 版本中强制使用小写。

## HTML 属性

属性为元素提供附加信息。属性总是以名称/值对的形式出现，比如：name=”value”。
属性总是在 HTML 元素的开始标签中规定。比如HTML 链接。

属性注意事项：

### 使用小写属性
始终为属性值加引号。双引号是最常用的，不过使用单引号也没有问题。
在某些个别的情况下，比如属性值本身就含有双引号，那么您必须使用单引号，例如：

name=’Bill “HelloWorld” Gates’

举几个栗子：

属性例子 1:

< h1> 定义标题的开始。
< h1 align="center"> 拥有关于对齐方式的附加信息。
TIY : 居中排列标题

属性例子 2：
<body> 定义 HTML 文档的主体。
<body bgcolor="yellow"> 拥有关于背景颜色的附加信息。
TIY : 背景颜色

属性例子 3:
<table> 定义 HTML 表格
<table border="1"> 拥有关于表格边框的附加信息。

## HTML 样式

当浏览器读到一个样式表，它会按照这个样式表来对文档进行格式化。有以下三种方式插入样式表：

## 外部样式表

当样式被应用于许多页面的时候，使用外部样式表。使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。如：

<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>

## 内部样式表

当单个文件需要特别样式时，就可以使用内部样式表。你可以在 head 部分通过 < style > 标签定义内部样式表。

<head>
<style type="text/css"> body {background-color: red}   p {margin-left: 20px}
</style>
</head>

## 内联样式

当特殊的样式需要应用到个别元素时，就可以使用内联样式。 使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何 CSS 属性。以下实例显示出如何改变段落的颜色和左外边距。

<p style="color: red; margin-left: 20px">
This is a paragraph
</p>