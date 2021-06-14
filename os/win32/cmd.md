
# cmd

cmd是一个shell

powershell(pwsh)也是一个shell

conhost.exe是一个终端





## 常用命令

### -c

-c 表示执行单行命令

比如
```powershell
cmd -c dir
pwsh -c ls
sh -c ls
python -c "print('hello')"

```

### mklink

要管理员权限，pwsh不可用

```bat
mklink test.py.lnk "E:\test.py"
为 test.py.lnk <<===>> E:\test.py 创建的符号链接
```

pwsh可以这样执行cmd命令:

```bat
cmd /c mklink
创建符号链接。

MKLINK [[/D] | [/H] | [/J]] Link Target

        /D      创建目录符号链接。默认为文件
                符号链接。
        /H      创建硬链接而非符号链接。
        /J      创建目录联接。
        Link    指定新的符号链接名称。
        Target  指定新链接引用的路径
                (相对或绝对)。
```

windows的.lnk也是二进制文件，不像linux中的链接只是一个符号，这涉及到文件系统(win7以上是NTFS3.1，linux4以上是ext4)的设计。

windows没有为右键创建快捷方式提供命令行方式，不过提供了API，你可以通过Windows API用C/C++编程创建快捷方式。

## 其他

