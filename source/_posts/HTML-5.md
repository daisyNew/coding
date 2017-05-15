---
title: HTML 5 
date: 2017-03-02 11:07:14
tags: html
---

### HTML 5 视频

直到现在，仍然不存在一项旨在网页上显示视频的标准。今天，大多数视频是通过插件（比如 Flash）来显示的。然而，并非所有浏览器都拥有同样的插件。

HTML5 规定了一种通过 video 元素来包含视频的标准方法。

#### 视频格式
当前，video 元素支持三种视频格式：

格式	  |  IE	  | Firefox |  Opera	|   Chrome	 |Safari
----- | ----- | ------ | ------    | --------   | ----------
Ogg	  |  No	  |3.5+	     |10.5+	   | 5.0+	  | No
MPEG4 |  9.0+ |  No	     |  No	   | 5.0+	 | 3.0+
WebM  |  No	  | 4.0+	 | 10.6+   | 6.0+	  | No

Ogg = 带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件
MPEG4 = 带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件
WebM = 带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件


#### h5中设置视频

``` html
<video src="movie.ogg" width="320" height="240" controls="controls">
Your browser does not support the video tag.
</video>
``` 

如果有的浏览器不支持video标签，就会显示标签对之间的那句话。

此外，video 元素允许多个 source 元素。source 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式：

``` html
<video width="320" height="240" controls="controls">
  <source src="movie.ogg" type="video/ogg">
  <source src="movie.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>
``` 

### h5 音频

#### 音频格式
当前，audio 元素支持三种音频格式：

 
  格式      |  IE 9	  |  Firefox 3.5 |  Opera 10.5 |  Chrome 3.0  | Safari 3.0
--------   | ------- | ----------   | ------------ | ------------ | ------------
Ogg Vorbis |	 	  |     √	     |       √	     |       √     | 
MP3	       |    √	  | 	 	     |              |        √     |     √
Wav	 	   |          |     √	     |      √	     |              |      √


#### h5中设置音频

``` html
<audio src="song.ogg" controls="controls">
Your browser does not support the audio tag.
</audio>
``` 








