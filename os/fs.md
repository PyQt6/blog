
# 文件系统 FileSystem

在win32中，目前是 NTFS3.1
在linux中，目前是 ext4

在linux中，目录也是文件，外设也是文件。下文中的"文件"指常规文件，包含目录等的"文件"会用"文件(包括目录)"标出。

同一目录下不能有任意两个`文件、目录、文件和目录`同名

## 路径 path

很多人都搞不清路径的问题。

### 路径示例
在win32中，类似于 D:\\1\\2\\3.txt
在linux中，类似于 /usr/bin/python3

### 路径分隔符
在win32中，默认是 \\ ，但 / 也可以用，且 \\ 和 / 可以混用，比如D:\\1/2\\3.txt
在linux中，是 /

### 路径格式
在win32中，是 unicode(utf-16)，对应C语言中的wchar_t，限制长度为260个wchar_t，不区分大小写，'\/:*?"<>|'不能出现在文件名(包括目录名)中
在linux中，是 utf-8，对应C语言中的char，限制长度为1024个char，区分大小写，只有'/:*?'不能出现在文件名(包括目录名)中。很多linux系统为了便于文件复制到其他系统，也不允许使用'\"<>|'

### 根目录
在win32中，没有根目录，每个分区的根目录是 C:\\,D:\\,...
在linux中，是 /

### 链接(link,ln)

软链接 ln -s 
硬链接 ln 

### 三种路径
- 绝对路径(absolute path, abspath) 允许含软链接，形如 /usr/bin或C:\\Windows\\System32
- 相对路径(relative path) 允许含软链接，形如 ./1.txt 或 ../a/b.txt
- 仅文件名(filename only) 没有路径前缀，只有一个文件名，形如 ping

> 真实路径(realpath) 是不含软链接的绝对路径，是绝对路径的一种特例，例如 /etc/passwd

### 用户目录 
在win32(cmd)中，是 %USERPROFILE%
在linux(shell)中，是 ~
在powershell中，是 $HOME

### 系统目录
在win32(cmd)中，是 %systemroot%，即C:\\Windows
在linux(shell)中，是 /system

### 常用目录

win32常用目录(很多应用会把它的配置文件放在这里)：
%USERPROFILE%/AppData/Local
%USERPROFILE%/AppData/Roaming
%USERPROFILE%/AppData/Local/Programs (这是Python和VSCodium的默认安装路径)
%USERPROFILE%/AppData/Local/Temp (这是win32的临时文件存放路径)
%USERPROFILE%/Desktop (桌面)
C:\\Windows (系统路径)
C:\\Windows\\System32 (系统程序路径)

linux常用目录：
/etc (系统配置文件所在目录)
/usr/bin (安装的应用所在的目录)
~/Desktop (桌面)
/system (系统路径)
/system/bin (系统程序路径)


### 当前工作目录(Current Working Directory,cwd)
在win32(cmd)中，是 .\\
在linux(shell)中，是 ./
在powershell中，是 $PWD

由于Linux命令pwd(print working directory)，cwd常被写成pwd

通常 当前工作目录 ≠ 程序执行时所在目录 ≠ 源文件所在目录

### 上级目录(父目录,parent directory)
在win32(cmd)中，是 ..\\
在linux(shell)中，是 ../

### OS的路径搜寻规则：
对于绝对路径和相对路径，搜寻位置是唯一的

比如：
绝对路径 /etc/passwd 就是从根目录开始找etc目录下的passwd
相对路径 ./a/b.txt 就是从$PWD开始找a目录下的b.txt
相对路径 ../a/b.txt 就是从$PWD的上级目录开始找a目录下的b.txt



对于仅文件名，规则是：
先找$PWD，再找系统path环境变量(一个个顺序找过去)。  
注意，这只适用于一条命令的第0个参数(argv[0],通常是程序,linux系统中也可以是".sh",".py",".pl"等脚本)和.dll(linux中是.so，python是.pyd)等动态链接库的搜寻。命令的后续参数被解释为纯字符串，不适用于这个规则。

需要说明的是，在程序运行时，会生成一个path环境变量的副本，在此程序中涉及的仅文件名搜寻是依据这个副本的，你可以对其任意修改来实现动态修改仅文件名搜寻路径。这样做的好处是不用修改系统配置中的path环境变量，更加安全。  

例如，要添加桌面到path环境变量中：
```python
# cmd
set PATH=%PATH%;%USERPROFILE%/Desktop;

# powershell
$env:path+=";$HOME/Desktop;"

# python
import os
os.environ["path"]+=";%USERPROFILE%/Desktop;"

```
linux要把;换成:

前后都有;是防止之前的path末尾没有;导致前后连在一起

### path环境变量

用于OS的 仅文件名搜寻路径

含多个路径时：
在win32中，用 ; 分隔
在linux中，用 : 分隔

### 隐藏的文件
linux是在前面在'.'
win32可以通过右键设置

### 编程语言自己的模块搜寻路径

不同于path环境变量，这是编程语言自己的模块搜寻路径。

python是 sys.path，可以通过sys.path.append(r"")来添加

julia是 LOAD_PATH，可以通过push!(LOAD_PATH,raw"")来添加

lua是 package.path，可以通过package.path=package.path..";lualib\\?.lua;"来添加

js没有模块搜寻路径，它直接使用绝对路径和相对路径来import文件，一个文件就是一个模块。当js使用import后面只有一个名称(而不是路径)时，它搜寻的是`(当前项目目录 以及 npm安装目录)`下的`(node_modules目录)`中的`(包的package.json)`中`("export"指定的入口点)`。(很绕，但是解释得很准确，简单理解就是`在node_modules目录下找符合该名称的包`)

动态语言一般都可以任意修改该变量来实现动态修改模块搜寻路径

## 常用命令

```sh
ls -a
ls -l
```

## 常用快捷键

按住Ctrl时拖动可以复制
按住Ctrl时双击或回车可以在新窗口打开



