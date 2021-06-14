

# ffmpeg 视频格式转换

```bat
ffmpeg -y -i %1 -vcodec copy -acodec copy "%~n1_output_MP4.mp4"
pause
```

或

```bat
ffmpeg -i %1 -vcodec h264 -acodec aac %1.mp4
pause
```

可以去看看github上的BAT_FFMPEG



# ffmpeg 剪视频

## 极简版剪视频

```powershell

# 00:01:00.000 -> 00:25:00.000
ffmpeg -ss 00:01:00.000 -t 00:24:00.000 -y -accurate_seek -i "E:/input.mkv" -vcodec copy -acodec copy -avoid_negative_ts 1 "E:/output.mkv"

# 00:00:00.000 -> 00:35:00.000
ffmpeg -ss 00:00:00.000 -to 00:35:00.000 -y -accurate_seek -i "E:/input.mkv" -vcodec copy -acodec copy "E:/output.mkv"

```

时间格式：

- s(只有秒) 0
- HOURS:MM:SS 00:00:00
- HOURS:MM:SS.MICROSECONDS 00:00:00.000

-y    overwrite output files 可以不加   
-vcodec copy -acodec copy 可以简写为 -codec copy    
-avoid_negative_ts 1 通常不加

参数分析：

    -i : source
    -ss: start time
    -t :duration
    -c :video,audio codec
    -to : time_stop

## ffmpeg视频精准剪切

### 1. 导言
ffmepg剪切视频，很方便，但是也有很大缺陷：  
（1）剪切时间点不精确   
（2）有时剪切的视频开头有黑屏

造成这些问题的原因是ffmpeg无法seek到非关键帧上。
一下本文通过一些参数配置尽可能地减轻以上问题

### 2. 基本剪切方法(不推荐使用)
```
ffmpeg -i test.mp4 -ss 10 -t 15 -codec copy cut.mp4
```

参数分析：

    -i : source
    -ss: start time
    -t :duration
    -c :video,audio codec
    -to : time_stop

时间格式：

- x秒
- HOURS:MM:SS.MICROSECONDS

可以设置输出视频的编码格式

    -vcodec xxx
    -acodec xxx

把-ss, -t参数放在-i参数之后，是对输出文件执行的seek操作     
输入文件会逐帧解码，直到-ss设置的时间点为止，这么操作会很慢，虽然时间点是准确的，但是很容易出现黑屏问题。

### 3. 参数优化
（1）将-ss， -t 参数放在-i参数之前

```
ffmpeg -ss 10 -t 15 -i test.mp4 -codec copy cut.mp4
```
对输入文件执行seek操作，会seek到-ss设置的时间点前面的关键帧上。     
时间不精确，但是不会出现黑屏

（2）accurate_seek
剪切时间更加精确
```
ffmpeg -ss 10 -t 15 -accurate_seek -i test.mp4 -codec copy cut.mp4
```
PS：accurate_seek必须放在-i参数之前

（3）avoid_negative_ts
如果编码格式采用的copy 最好加上 -avoid_negative_ts 1参数
```
ffmpeg -ss 10 -t 15 -accurate_seek -i test.mp4 -codec copy -avoid_negative_ts 1 cut.mp4
```

### 4. 参考文献
[1] http://trac.ffmpeg.org/wiki/Seeking






## 使用FFmpeg合并MP4视频 (转载)

1. 使用mpeg拼接
2. ==使用ts拼接==


使用FFmpeg合并MP4视频
windows/linux下均可用

1. 使用mpeg拼接
速度慢，文件大

将 mp4 先转码为 mpeg文件，mpeg是支持简单拼接的，然后再转回 mp4。
```
ffmpeg -i 1.mp4 -qscale 4 1.mpg
ffmpeg -i 2.mp4 -qscale 4 2.mpg
cat 1.mpg 2.mpg | ffmpeg -f mpeg -i - -qscale 6 -vcodec mpeg4 output.mp4
```
2. ==使用ts拼接==
速度快，文件小

先将 mp4 转化为同样编码形式的 ts 流，因为 ts流是可以 concate 的，先把 mp4 封装成 ts ，然后 concate ts 流， 最后再把 ts 流转化为 mp4。
```
ffmpeg -i 1.mp4 -vcodec copy -acodec copy -vbsf h264_mp4toannexb 1.ts
ffmpeg -i 2.mp4 -vcodec copy -acodec copy -vbsf h264_mp4toannexb 2.ts
ffmpeg -i "concat:1.ts|2.ts" -acodec copy -vcodec copy -absf aac_adtstoasc output.mp4
```

## 使用ffmpeg裁剪和合并视频 (转载)

剪切视频

使用 -ss 和 -t 选项，从第0秒开始，向后截取31秒视频，并保存
```
ffmpeg -ss 00:00:00 -i video.mp4 -vcodec copy -acodec copy -t 00:00:31 output1.mp4
```
从第01:33:30 开始，向后截取 00:47:16 视频，并保存
```
ffmpeg -ss 01:33:30 -i video.mp4 -vcodec copy -acodec copy -t 00:47:16 output2.mp4
```
合并视频

把剪切得到的两个视频合并成一个视频

使用 TS格式拼接视频

先将 mp4 转化为同样编码形式的 ts 流，因为 ts流是可以 concate 的，先把 mp4 封装成 ts ，然后 concate ts 流， 最后再把 ts 流转化为 mp4。


```
ffmpeg -i output1.mp4 -vcodec copy -acodec copy -vbsf h264_mp4toannexb output1.ts
ffmpeg -i output2.mp4 -vcodec copy -acodec copy -vbsf h264_mp4toannexb output2.ts
```

为了减少命令的输入，需要一个filelist.txt文件，里面内容如下

```
file 'output1.ts'
file 'output2.ts'
```

合并视频命令
    
```
ffmpeg -f concat -i filelist.txt -acodec copy -vcodec copy -absf aac_adtstoasc output.mp4
```

## 简单版剪视频

假设一个视频文件有28分钟15秒，现在需要剪辑成2个视频文件，第1个15分钟，第2个是剩下的时间长度

```
ffmpeg -ss 0 -t 900 -accurate_seek -i input.mp4 -codec copy -avoid_negative_ts 1 output1.mp4
ffmpeg -ss 895 -accurate_seek -i input.mp4 -codec copy -avoid_negative_ts 1 output2.mp4
```
第2个视频设置了从895秒开始，这样2个视频有上下承接的5秒，用户看起来是连贯的。



用ffmepg在视频前面删除固定长度是非常方便的，因为提供了直接就可以使用的参数。但是要批量截去视频结尾固定长度的视频，却比较棘手。

网络上提供的版本无一不是先用ffprobe返回视频的长度，然后将该长度减去固定时长，作为参数再返回给ffmpeg调用。


## ffmpeg剪辑视频 (转载)


ffmpeg剪辑视频文件非常简单，一个命令就可以搞定。
```
ffmpeg -ss 00:03:00 -i video.mp4 -t 60 -c copy cut.mp4
```
-ss后面指定的时间轴，-t后面指定时长单位为秒。为什么要将-ss放在-i前面？因为官方文档推荐这样做，这样做剪辑出来的视频时间轴更精准，并且速度更快。还有一个参数-to放在-i video.mp4后面，作用是指定剪辑时长，例如-to 00:02:00，当-ss放在-i前面的时候，这个-to剪辑出来的是-ss指定的时间轴加上-to指定的时间，比如-ss 00:01:00 -i video.mp4 -to 00:02:00，则剪辑出来的视频，是原视频00:01:00到00:03:00的片段。如果想把片头给去掉则指定了时间轴就不要添加-to和-t参数。

-to参数实例
```
ffmpeg -ss 00:03:00 -i video.mp4 -to 00:02:00 -c copy cut.mp4
```
以上命令代表将原视频文件00:03:00到00:05:00的片段剪辑出来，生成为cut.mp4文件在当前文件夹，并且使用编码为copy复制源视频文件的编码格式。

去除片头
```
ffmpeg -ss 00:03:00 -i video.mp4 -c copy cut.mp4
```
去除片头，就不需要添加-to或者-t参数，那么则是剪辑00:03:00到视频结尾

1. 分离视频音频流
ffmpeg -i input_file -vcodec copy -an output_file_video　　//分离视频流
ffmpeg -i input_file -acodec copy -vn output_file_audio　　//分离音频流
2. 视频解复用
ffmpeg –i test.mp4 –vcodec copy –an –f m4v test.264
ffmpeg –i test.avi –vcodec copy –an –f m4v test.264
3. 视频转码
ffmpeg –i test.mp4 –vcodec h264 –s 352*278 –an –f m4v test.264              //转码为码流原始文件
ffmpeg –i test.mp4 –vcodec h264 –bf 0 –g 25 –s 352*278 –an –f m4v test.264  //转码为码流原始文件
ffmpeg –i test.avi -vcodec mpeg4 –vtag xvid –qsame test_xvid.avi            //转码为封装文件
//-bf B帧数目控制，-g 关键帧间隔控制，-s 分辨率控制
4. 视频封装
ffmpeg –i video_file –i audio_file –vcodec copy –acodec copy output_file
5. 视频剪切
ffmpeg –i test.avi –r 1 –f image2 image-%3d.jpeg        //提取图片
ffmpeg -ss 0:1:30 -t 0:0:20 -i input.avi -vcodec copy -acodec copy output.avi    //剪切视频
//-r 提取图像的频率，-ss 开始时间，-t 持续时间
6. 视频录制
ffmpeg –i rtsp://192.168.3.205:5555/test –vcodec copy out.avi
7. YUV序列播放
ffplay -f rawvideo -video_size 1920x1080 input.yuv
8. YUV序列转AVI
ffmpeg –s w*h –pix_fmt yuv420p –i input.yuv –vcodec mpeg4 output.avi
9. 视频转帧序列
ffmpeg -i split.avi %d.bmp
10. 帧序列合并为视频
ffmpeg -i %d.bmp -y list.mp4

.获取视频流信息 
用ffprobe可以获取到视频的所有流的具体信息

ffprobe -print_format json -show_streams -i input.mp4
1
2.多个视频拼接 
可以将几个视频拼接成一个视频 -f 表示采用concat协议，-c 表示采用什么编码器 copy表示不重新编码，如果是x264 表示将采用x264进行重新编码。

ffmpeg -y -f concat -i videolist.txt -c copy  output.mp4
1
3.视频截图 
截一张图 
-ss 表示在视频的多少S 截取一张图

ffmpeg -y -ss 8 -i input.mp4 -f image2 -vframes 1 output.jpg
1
截多张图 
-r 表示每秒截多少张图； -qscale 表示生成的截图质量，该值越小图片质量越好；%5d.jpg 表示生成的截图的命令规则，5位数的整数命名。

ffmpeg -y -ss 0 -i input.mp4 -f image2  -r 1 -t 8 -qscale 1 ./jpgs/%5d.jpg
1
4.给视频加上水印图片

ffmpeg -y -i input.mp4  -i ./logo.png filter_complex "overlay=0:0:enable=between(t,0,2)" -c:v libx264 -c:a aac -strict -2 output.mp4
1
5.图片合成视频

ffmpeg -y -f image2 -framerate 10 -i ./jpgs/%05d.jpg -c:v libx264 -r 25 -pix_fmt yuv420p output.mp4
1
6.视频添加背景音乐

ffmpeg -y -i input.mp4 -i ainiyiwannian.wav -filter_complex "[0:a] pan=stereo|c0=1*c0|c1=1*c1 [a1], [1:a] pan=stereo|c0=1*c0|c1=1*c1 [a2],[a1][a2]amix=duration=first,pan=stereo|c0<c0+c1|c1<c2+c3,pan=mono|c0=c0+c1[a]" -map "[a]" -map 0:v -c:v libx264 -c:a aac -strict -2 -ac 2 output.mp4
1
7.将视频去除音频

ffmpeg -y -i source.mp4 -an -vcodec copy output.mp4
1
8.设置视频的音量 
-vol 设置视频的音量，是以%为单位，500表示500%

ffmpeg -y -i source.mp4 -vol 500 -strict -2 -vcodec copy output.mp4
1
9.视频转码 
-vcodec 指定视频编码器，-acodec 指定音频编码器
ffmpeg -y -i input.mp4 -vcodec libx264 -acodec copy output.mp4
音频处理

1.从视频中提取音频
ffmpeg -y -i source.mp4 -vn output.wav
2.将音频用lpcm格式重新编码，指定采样率
ffmpeg -y -i source.wav -acodec pcm_s16le -ar 44100 output.wav

视频打多个水印：


 ffmpeg -i 1.mp4 -i logo.png -i xh9.png -filter_complex "overlay=5:5,overlay=x=main_w-overlay_w-10 : main_h-overlay_h-10" 11.mp4 -y


水印图片位置
overlay值
左上角
10:10
右上角
main_w-overlay_w-10:10
左下角
10:main_h-overlay_h-10
右下角
main_w-overlay_w-10 : main_h-overlay_h-10



## ffmpeg调整缩放裁剪视频的基础知识 (转载)

### 1.resize and scale video 调整视频的大小和尺寸

　　1-1.调整视频大小(resize)是改变视频的宽度和高度。

　　　　　　使用-s参数实现，语法：ffmpeg  -i  input_file  -s  wxh  output_file (wxh是宽x高，比如320x240)

　　　  调整视频的尺寸(scale)是改变帧的数量。

　　　　　　![img](assets/1020339-20170629160701383-1292572427.png)

　　　　　　![img](assets/1020339-20170629160539836-1027551821.png)

　　1-2.预定义的视频大小简写如下：

　　　　![img](assets/1020339-20170629153918899-557975503.png)

　　　　![img](assets/1020339-20170629154024227-332229371.png)

 

### 2.视频裁剪

　　![img](assets/1020339-20170630113241321-1501458510.png)

　　视频裁剪使用crop视频滤镜，它可以把视频从指定的x、y位置裁剪成指定的w、h。坐标系是基于左上点开始的。

　　语法如下：

　　　　![img](assets/1020339-20170630114029758-332741423.png)

　　example：

　　　　![img](assets/1020339-20170630114255977-774259027.png)

　　如果不指定裁剪的开始点，则x、y的默认值为

　　　　![img](assets/1020339-20170630115719352-183427077.png)

 

　　　　example：ffmpeg  -i  intput.avi  -vf  crop=iw/2:ih/2  output.avi 

　　需要指定裁剪时长，使用 -t 参数，比如 -t 10 表示只裁剪10秒钟。





