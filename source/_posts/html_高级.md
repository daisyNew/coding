---
title: HTML 高级
date: 2016-10-16
---

## HTML Object 元素

< object> 的作用是支持 HTML 助手（插件）。

## HTML 助手（插件）

辅助应用程序（helper application）是可由浏览器启动的程序。辅助应用程序也称为插件。

辅助程序可用于播放音频和视频（以及其他）。辅助程序是使用 标签来加载的。

使用辅助程序播放视频和音频的一个优势是，您能够允许用户来控制部分或全部播放设置。

大多数辅助应用程序允许对音量设置和播放功能（比如后退、暂停、停止和播放）的手工（或程序的）控制。

使用 QuickTime 来播放 Wave 音频。实例：

``` html
< object width="420" height="360"
classid="clsid:02BF25D5-8C17-4B23-BC80-D3488ABDDC6B"
codebase="http://www.apple.com/qtactivex/qtplugin.cab">
<param name="src" value="bird.wav" />
<param name="controller" value="true" />
</object>
``` 

使用 QuickTime 来播放 MP4 视频。实例：

``` html
< object width="420" height="360"
classid="clsid:02BF25D5-8C17-4B23-BC80-D3488ABDDC6B"
codebase="http://www.apple.com/qtactivex/qtplugin.cab">
<param name="src" value="movie.mp4" />
<param name="controller" value="true" />
</object>
``` 

使用 Flash 来播放 SWF 视频。实例：

``` html
< object width="400" height="40"
classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000"
codebase="http://fpdownload.macromedia.com/
pub/shockwave/cabs/flash/swflash.cab#version=8,0,0,0">
<param name="SRC" value="bookmark.swf">
<embed src="bookmark.swf" width="400" height="40"></embed>
</object>
``` 

使用 Windows Media Player 来播放 WMV 影片
下面的例子展示用于播放 Windows 媒体文件的推荐代码：

``` html
     <object width="100%" height="100%"
     type="video/x-ms-asf" url="3d.wmv" data="3d.wmv"
     classid="CLSID:6BF52A52-394A-11d3-B153-00C04F79FAA6">
     <param name="url" value="3d.wmv">
     <param name="filename" value="3d.wmv">
     <param name="autostart" value="1">
     <param name="uiMode" value="full" />
     <param name="autosize" value="1">
     <param name="playcount" value="1">
     <embed type="application/x-mplayer2" src="3d.wmv" width="100%"
 height="100%" autostart="true" showcontrols="true" 
pluginspage="http://www.microsoft.com/Windows/MediaPlayer/"></embed>
     </object>
     ``` 
## HTML 播放音频

``` html
< audio controls="controls" height="100" width="100">
  <source src="song.mp3" type="audio/mp3" />
  <source src="song.ogg" type="audio/ogg" />
<embed height="100" width="100" src="song.mp3" />
</audio>
``` 

上面的例子使用了两个不同的音频格式。HTML5 < audio> 元素会尝试以 mp3 或 ogg 来播放音频。如果失败，代码将回退尝试 < embed> 元素。

问题：

您必须把音频转换为不同的格式。
< audio> 元素无法通过 HTML 4 和 XHTML 验证。
< embed> 元素无法通过 HTML 4 和 XHTML 验证。
< embed> 元素无法回退来显示错误消息。

## 雅虎的媒体播放器

雅虎播放器能播放 mp3 以及一系列其他格式。通过一行简单的代码，您就可以把它添加到网页中，轻松地将 HTML 页面转变为专业的播放列表。实例：

``` html
< a href="song.mp3">Play Sound</a>
< script type="text/javascript" src="http://mediaplayer.yahoo.com/js">
</script>
```

使用雅虎播放器是免费的。如需使用它，您需要把这段 JavaScript 插入网页底部：

``` html
< script type="text/javascript" src="http://mediaplayer.yahoo.com/js"></script>
```

然后只需简单地把 MP3 文件链接到您的 HTML 中，JavaScript 会自动地为每首歌创建播放按钮：

``` html
< a href="song1.mp3">Play Song 1</a>
< a href="song2.mp3">Play Song 2</a>
...
...
...
```

雅虎媒体播放器为您的用户提供的是一个小型的播放按钮，而不是完整的播放器。不过，当您点击该按钮，会弹出完整的播放器。
请注意，这个播放器始终停靠在窗框底部。只需点击它，就可将其滑出。

## HTML 播放视频

``` html
< video width="320" height="240" controls="controls">
< source src="movie.mp4" type="video/mp4" />
< source src="movie.ogg" type="video/ogg" />
< source src="movie.webm" type="video/webm" />
< object data="movie.mp4" width="320" height="240">
< embed src="movie.swf" width="320" height="240" />
< /object>
< /video>
``` 

上例中使用了 4 中不同的视频格式。HTML 5 < video> 元素会尝试播放以 mp4、ogg 或 webm 格式中的一种来播放视频。如果均失败，则回退到 < embed> 元素。

问题：

您必须把视频转换为很多不同的格式
< video> 元素无法通过 HTML 4 和 XHTML 验证。
< embed> 元素无法通过 HTML 4 和 XHTML 验证。

## 优酷解决方案

在 HTML 中显示视频的最简单的方法是使用优酷等视频网站。
如果您希望在网页中播放视频，那么您可以把视频上传到优酷等视频网站，然后在您的网页中插入 HTML 代码即可播放视频：

``` html
    <embed src="http://player.youku.com/player.php/sid/XMzI2NTc4NTMy/v.swf" 
width="480" height="400" 
type="application/x-shockwave-flash">
    </embed>
``` 