---
title: HTML 基础（二）
date: 2016-09-20 
---

我们通过使用 < a > 标签在 HTML 中创建链接。

有两种使用 < a > 标签的方式：

通过使用 href 属性 - 创建指向另一个文档的链接
通过使用 name 属性 - 创建文档内的书签

href 属性规定链接的目标。

开始标签和结束标签之间的文字被作为超级链接来显示。

## 实例：

< a href="http://www.w3school.com.cn/">Visit W3School</a>
提示：”链接文本” 不必一定是文本。图片或其他 HTML 元素都可以成为链接。

## HTML 链接 - target 属性

使用 Target 属性，你可以定义被链接的文档在何处显示。
下面的这行会在新窗口打开文档：

< a href="http://www.w3school.com.cn/"target="_blank">Visit W3School!</a>

## HTML 链接 - name 属性

name 属性规定锚（anchor）的名称。我们可以使用 name 属性来创建 HTML 页面中的书签。

当使用命名锚（named anchors）时，我们可以创建直接跳至该命名锚（比如页面中某个小节）的链接，这样使用者就无需不停地滚动页面来寻找他们需要的信息了。
命名锚的语法：

<a name="label">锚（显示在页面上的文本）</a>
提示：

锚的名称可以是任何你喜欢的名字。
您可以使用 id 属性来替代 name 属性，命名锚同样有效。

## HTML 图像

在 HTML 中，图像由 < img> 标签定义。< img> 是空标签，意思是说，它只包含属性，并且没有闭合标签。要在页面上显示图像，你需要使用源属性（src）。src 指 “source”。源属性的值是图像的 URL 地址。

定义图像的语法是：

<img src="url" />
URL 指存储图像的位置。浏览器将图像显示在文档中图像标签出现的地方。

替换文本属性（Alt）

alt 属性用来为图像定义一串预备的可替换的文本。替换文本属性的值是用户定义的。

<img src="boat.gif" alt="Big Boat">
在浏览器无法载入图像时，替换文本相当于一个占位信息。

## HTML 表格

表格由 < table> 标签来定义。每个表格均有若干行（由 < tr> 标签定义），每行被分割为若干单元格（由 < td> 标签定义）。字母 td 指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等。例如：

<table border="1">
<tr>
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
</tr>
</table>

表格和边框属性

如果不定义边框属性，表格将不显示边框。有时这很有用，但是大多数时候，我们希望显示边框。
使用边框属性来显示一个带有边框的表格：

<table border="1">
<tr>
<td>Row 1, cell 1</td>
<td>Row 1, cell 2</td>
</tr>
</table>
表格的表头

表格的表头使用 标签进行定义。
大多数浏览器会把表头显示为粗体居中的文本：

<table border="1">
<tr>
<th>Heading</th>
<th>Another Heading</th>
</tr>
<tr>
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
</tr>
</table>

## HTML 列表

### 无序列表

无序列表是一个项目的列表，此列项目使用粗体圆点（典型的小黑圆圈）进行标记。无序列表始于 < ul> 标签。每个列表项始于 < li>。

<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>

### 有序列表

同样，有序列表也是一列项目，列表项目使用数字进行标记。
有序列表始于 < ol> 标签。每个列表项始于 < li> 标签。

<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>

### 定义列表

自定义列表不仅仅是一列项目，而是项目及其注释的组合。
自定义列表以 < dl> 标签开始。每个自定义列表项以 < dt> 开始。每个自定义列表项的定义以 < dd> 开始。

<dl>
<dt>Coffee</dt>
<dd>Black hot drink</dd>
<dt>Milk</dt>
<dd>White cold drink</dd>
</dl>

## HTML 块元素

大多数 HTML 元素被定义为块级元素或内联元素。
块级元素在浏览器显示时，通常会以新行来开始（和结束）。
内联元素在显示时通常不会以新行开始。

## HTML < div> 元素

HTML < div> 元素是块级元素，它是可用于组合其他 HTML 元素的容器。
< div> 元素没有特定的含义。除此之外，由于它属于块级元素，浏览器会在其前后显示折行。
如果与 CSS 一同使用，< div> 元素可用于对大的内容块设置样式属性。
< div> 元素的另一个常见的用途是文档布局。它取代了使用表格定义布局的老式方法。使用 < table> 元素进行文档布局不是表格的正确用法。< table> 元素的作用是显示表格化的数据。

## HTML 元素

HTML < span> 元素是内联元素，可用作文本的容器。
< span> 元素也没有特定的含义。
当与 CSS 一同使用时，< span> 元素可用于为部分文本设置样式属性。

## HTML 表单

表单是一个包含表单元素的区域。表单元素是允许用户在表单中（比如：文本域、下拉列表、单选框、复选框等等）输入信息的元素。
表单使用表单标签（< form>）定义：

<form>
 ...
  input 元素
 ...
</form>
多数情况下被用到的表单标签是输入标签（< input>）。输入类型是由类型属性（type）定义的。大多数经常被用到的输入类型如下：

## 文本域（Text Fields）

<form>
First name: 
<input type="text" name="firstname" />
<br />
Last name: 
<input type="text" name="lastname" />
</form>
注意，表单本身并不可见。同时，在大多数浏览器中，文本域的缺省宽度是20个字符。

## 单选按钮（Radio Buttons）

当用户从若干给定的的选择中选取其一时，就会用到单选框。

<form>
<input type="radio" name="sex" value="male" /> Male
<br />
<input type="radio" name="sex" value="female" /> Female
</form>
注意，只能从中选取其一。

## 复选框（Checkboxes）

当用户需要从若干给定的选择中选取一个或若干选项时，就会用到复选框。

<form>
<input type="checkbox" name="bike" />
I have a bike
<br />
<input type="checkbox" name="car" />
I have a car
</form>
表单的动作属性（Action）和确认按钮

当用户单击确认按钮时，表单的内容会被传送到另一个文件。表单的动作属性定义了目的文件的文件名。由动作属性定义的这个文件通常会对接收到的输入数据进行相关的处理。

<form name="input" action="html_form_action.asp"
method="get">
Username: 
<input type="text" name="user" />
<input type="submit" value="Submit" />
</form>

## 下拉列表

<form>
<select name="cars">
<option value="volvo">Volvo</option>
<option value="saab">Saab</option>
<option value="fiat">Fiat</option>
<option value="audi">Audi</option>
</select>
</form>

## HTML 背景

背景颜色（Bgcolor）
背景颜色属性将背景设置为某种颜色。属性值可以是十六进制数、RGB 值或颜色名。

<body bgcolor="#000000">
<body bgcolor="rgb(0,0,0)">
<body bgcolor="black">
背景（Background）
背景属性将背景设置为图像。属性值为图像的URL。如果图像尺寸小于浏览器窗口，那么图像将在整个浏览器窗口进行复制。

<body background="clouds.gif">
<body background="http://www.w3school.com.cn/clouds.gif">
