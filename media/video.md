
# 视频相关 Video
封装好的视频一般有多个轨道，一般是视频轨、音频轨、字幕轨等。  
无论是mkv还是mp4、avi、flv、rmvb等都是视频的封装格式，其第一个轨道才是真正的纯视频(无声音)。   

## 视频编码

常见纯视频使用的编码有h.264(avc)和h.265(hevc)

## 音频编码

常见的音频编码有aac和flac

## 字幕格式

常见的有srt,ass,ssa三种字幕

从级别上来说
srt < ssa < ass
SRT是最基本的字幕，通常只有字的文本内容和时间长度。
SSA是功能更完备的字幕，可以在SRT基础上加上字的大小、位置及水印等内容。
ASS是SSA字幕的升级版，功能更加强大，它除了包含SSA的所有好功能以外，还进行了扩展。
至于好坏方面不好说。
因为功能强大的却有结构复杂的毛病，且有时候在支持方面有问题。
而功能简单的SRT则可以在任何场合使用，适用范围很广，但效果单一。
如果想简单方便，就用SRT。
如果想华丽好看，就用SSA或者ASS。
其实还有比SRT更简单的字幕：LRC，这是千千静听都可以制作做的字幕，LRC比SRT更简单的原因是：它连字幕结尾的时间都省略掉了……

扩展：

字幕常见的标识chs、cht、GB、Big5、eng。

chs是Chinese Simplified的缩写，表示简体中文。

cht是Chinese Traditional的缩写，表示繁体中文。

GB既“国标”的汉语拼音的缩写，中华人民共和国国家标准的意思，简体中文。

Big5是在台湾和香港等地广为使用的计算机汉字编码方案，繁体中文。

eng是English的简写，也就是说表示英文。


### srt

全称是SubRip Text

最常见的文本字幕，制作起来简单，时间代码+字幕。

### ssa

全称是S Station Alpha

功能上比src更强大支持各种颜色、字体等特效。

### ass

全称是Advanced SubStation Alpha

包含ssa所有的功能并在ssa原有功能的基础上还进行了扩展。

视觉角度上讲ssa和ass视觉效果更好一些。


## mkv
mkv是Matroska多媒体容器文件使用的扩展名

Matroska多媒体容器（Multimedia Container）是一种开放标准的自由的容器和文件格式，是一种多媒体封装格式，能够在一个文件中容纳无限数量的视频、音频、图片或字幕轨道。所以其不是一种压缩格式，而是Matroska定义的一种多媒体容器文件。其目标是作为一种统一格式保存常见的电影、电视节目等多媒体内容。在概念上Matroska和其他容器，比如AVI、MP4或ASF（Advanced Streaming Format，即高级流格式）比较类似，但其在技术规程上完全开放，在实现上包含很多开源软件。可将多种不同编码的视频及16条以上不同格式的音频和不同语言的字幕流封装到一个Matroska 媒体文件当中。最大的特点就是能容纳多种不同类型编码的视频、音频及字幕流。

可以使用[mkvtoolnix](https://mkvtoolnix.download/downloads.html)对mkv进行提取、混流。

## 常用软件

[ffmpeg](https://github.com/FFmpeg/FFmpeg/releases) 可以用于视频信息提取、编码、解码、转码、播放。基于GPLv3或LGPL协议。


