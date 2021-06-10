
# 逆向 reverse


## 工具

- x64dbg

x64dbg内含x32dbg，可以调试64/32位程序。

- OllyDbg

OllyDbg通常称作OD，是以前反汇编工作的常用工具。由于只能调试32位程序，现在很少使用了。

- IDA Pro

静态反汇编工具。图标是世界上第一个程序员(女)。


## 汇编指令

注：以下汇编以nasm或x64dbg语法为例，nasm是大小写敏感的。如果你看到我把指令错写成了大写，请提醒我。

> cdq   
> idiv ecx

就是先进行指令双字扩展，把eax中的符号位扩展到edx中去，然后edx:eax对应的值除以ecx，商保存到eax中，余数保存到edx中。


> xor eax,eax;即eax=eax^eax

就是把eax清零(mov eax,0)


> lea eax,[ebx]

等价于mov eax,ebx


> dword ptr ds:[76CB10B0]

就是取出地址76CB10B0上的操作数，长度为dword(双字，x86最初是在8086处理器上使用的，字长为16bit，双字就是4字节。当然你现在的处理器字长肯定不止16bit，但约定dword=32bit=4字节)

- word=16bit=2字节
- dword=32bit=2字节
- qword=64bit=2字节
- tword=128bit=2字节


> dword ptr ss:[ebp+8]

就是寄存器间接寻址


## 具体

### 硬编码序列号

是最简单的序列号。

硬编码

> 在计算机程序或文本编辑中，硬编码是指将可变变量用一个固定值来代替的方法。用这种方法编译后，如果以后需要更改此变量就非常困难了。大部分程序语言里，可以将一个固定数值定义为一个标记，然后用这个特殊标记来取代变量名称。当标记名称改变时，变量名不变，这样，当重新编译整个程序时，所有变量都不再是固定值，这样就更容易的实现了改变变量的目的。尽管通过编辑器的查找替换功能也能实现整个变量名称的替换，但也很有可能出现多换或者少换的情况，而在计算机程序中，任何小错误的出现都是不可饶恕的。最好的方法是单独为变量名划分空间，来实现这种变化，就如同前面说的那样，将需要改变的变量名暂时用一个定义好的标记名称来代替就是一种很好的方法。

硬编码序列号不依赖于用户名来生成,总是由固定的字符和数字构成的固定不变的字符串,这种序列号通常比较容易找到。

#### 方法1

作为最简单的硬编码序列号的例子，序列号是作为全局字符串出现在程序中的。想要找到所有的全局字符串,我们在反汇编窗口中右键->Search for->All referenced text strings。我们可以看到全局字符串。

但是我们不推荐用查找全局字符串的方法定位序列号。复杂的程序引用的字符串有成千上万行，从其中查找出字符串消耗的时间太久。而有些程序也会故意放置一些假的序列号在字符串列表中，若是直接查找恰恰中了陷阱。

#### 方法2

通过查找相关的API函数来定位引用字符串的主要代码段。在反汇编窗口中右键->Search for->Name(label) in current module来查看API函数列表。

结合API函数列表推断出 GetDlgItemTextA/W 或 GetWindowTextA/W 获取用户输入的字符,MessageBoxA/W提示输入的字符正确与否。

给这两个函数下断点。

定位字符串存储的缓冲区：
我们观察此时的栈空间，程序中断在GetDlgItemTextA处，该函数用于获取用户输入的字符,并且该函数的Buffer参数是用于存放获取到的字符串的缓冲区首地址。我们在该参数上右键->Follow in Dump定位到缓冲区在数据窗口中的位置。

发现此时缓冲区并没有任何数据。接下来在函数的返回指令RETN对应的地址处按下F2下断点，按F9运行到该处再按F2取消断点，我们发现此时的缓冲区内保存了我们刚刚输入的错误序列号。

我们选中我们输入的字符串，下内存访问断点。

然后按F9继续运行，(若发现此时程序还是中断在API函数处，就继续按F9，)此时程序停在了我们设置的内存访问断点处。

后面只要分析序列号比较规则或序列号生成算法即可。

### 序列号生成算法

同上面方法2，后面分析序列号生成算法即可。

### 按钮不可用型程序的序列号破解 (以下是转载)

在License Name里面输入HELLO，在License Key里面输入TES，发现OK按钮是灰色的，然后返回OllyDbg窗口界面。

然后点击上方工具栏的M按钮进入内存窗口，右键-> Search（快捷键Ctrl+B），在ASCII那一栏输入我们刚刚在License Key里面输入TES，然后点击OK寻找。

程序自动寻找我们刚刚输入的TES字符，自动弹出已经定位在TES字符串的Dump窗口。我们选中TES字符串，右键->Search For->Next，发现程序在窗口下方出现黄色提示提示未找到其他条目,说明这就是我们刚刚输入的序列号。

记住TES对应的内存地址00BC5ED5（不同的电脑对应的地址可能不同，根据方法在实验中作相应替换即可），然后点击工具栏的C按钮返回CPU窗口，点击在下方的数据窗口，右键->Go to->Expression（快捷键Ctrl+G），在弹出的窗口中输入刚刚我们记住的地址00BC5ED5，点击OK。

为了进一步验证这是我们输入序列号的存放地址，我们切换到程序界面再输入一个T，然后切换回OllyDbg中观察数据窗口的00BC5ED5处，发现我们刚刚输入的T已经被保存，因此我们更加确定00BC5ED5就是存放我们输入的字符串的地址。

我们选中00BC5ED5之后六位（所要求的的License Key是六位），右键设置内存访问断点，这样程序运行到访问该内存时就会停下。

通过断点找到关键代码处，找到序列号
我们切换回程序注册窗口，再输入一个字符Y，发现程序中断在004029A1处，阅读这几行代码，发现这只是将输入存入00BC5ED5内存段处。我们继续按F9，程序停在下一处访问内存00BC5ED5处，我们发现程序还是中断在这附近，继续按一次F9。

我们可以发现此时0012E79C处是乱码，我们按F8，运行004B8E9C处的REP MOVS BYTE PTR ES:[EDI],BYTE PTR DS:[ESI]指令，发现我们输入的序列号已经被复制进0012E79C地址处。

此时我们输入的字符串存在于两个内存地址处，00BC5ED5和0012E79C处。00BC5ED5已经被下了内存断点，由于内存断点只能设置一个，因此我们给0012E79C内存中的字符串设置硬件访问断点。选中相应字符串，右键->Breakpoint->Hardware,on access->Dword设置硬件访问断点。

**我们给两个储存着字符串的地址都下了断点，进行序列号比对时至少会访问这两个内存地址的中的一个，因此我们只需要跟踪断点就可以定位到序列号进行比对的关键代码处。** 我们继续按F9运行。

我们可以发现程序中断在004031C4处，观察此时的寄存器窗口发现有一个之前从未出现过的字符串，因此我们可以猜测这一串字符串和我们需要查找的序列号有关。

> 在遇到某些特殊情况时，我们稍加分析就可以绕过程序设置的障碍。例如不可用按钮，序列号正确时按钮才可用，所以程序肯定还是要读入我们的输入，以便和正确的序列号对比。我们就可以以这一点为切入点，从而破解程序。


### 绕过检测OllyDbg进程名的反调试检测方法

反调试 程序会检测自身是否正在被调试,如果检测到正在被调试的话,就会结束自身进程或者不按常规流程运行。所以绕过程序对调试的检测是很有必要的。

IsDebuggerPresent 函数 IsDebuggerPresent是确定调用进程是否由用户模式的调试器调试。 语法：BOOL WINAPI IsDebuggerPresent(void); 该函数没有参数。如果当前进程运行在调试器的上下文，返回值为非零值；如果当前进程没有运行在调试器的上下文，返回值是零。

PID 每个进程在系统中都有唯一的一个ID标识它，这个ID就是进程标识符(PID)。因为其唯一，所以系统可以根据它准确定位到一个进程。进程标识符的类型为pid_t，其本质上是一个无符号整型的类型别名(typedef)。一个进程标识符唯一对应一个进程，而多个进程标识符可以对应同一个程序。

句柄 在程序设计中，句柄是一种特殊的智能指针 。当一个应用程序要引用其他系统（如数据库、操作系统）所管理的内存块或对象时，就要使用句柄。句柄与普通指针的区别在于，指针包含的是引用对象的内存地址，而句柄则是由系统所管理的引用标识，该标识可以被系统重新定位到一个内存地址上。这种间接访问对象的模式增强了系统对引用对象的控制。

GetProcAddress函数 GetProcAddress函数检索指定的动态链接库(DLL)中的输出库函数地址。如果函数调用成功，返回值是DLL中的输出函数地址。如果函数调用失败，返回值是NULL。得到进一步的错误信息，调用函数GetLastError。

TerminateProcess函数 终止指定进程及其所有线程。参数：hProcess：指定要中断进程的句柄. 该句柄可以由 OpenProcess得到；DWORD uExitCode：进程和其所有线程的退出代码。返回非零值代表成功。0代表失败。


关键：获取隐藏的API函数
从GetProcAddress函数中获取隐藏的函数

打开OllyDbg，载入程序,然后查找API函数列表，发现并没有检测进程名的API函数，这些重要的API函数被隐藏起来了。很显然如果程序不直接导入某些API的话,会使用GetProcAddress这个API 函数来获取这些API函数的地址进行间接调用。

给该函数下断点，然后返回CPU窗口按F9运行，可以发现程序中断在GetProcAddress函数的入口处，观察此时的堆栈窗口，可以看见该API函数载入函数的名字。


然后我们按F9运行，发现程序停在7C80AE30地址处，我们继续按F9，可以看到右下角堆栈窗口内的参数ProcNameOrOrdinal的值在变化。

接下来我们一直按下F9，直到当待获取的API函数是EnumProcesses时，这时我们在返回地址7C80AE8F处按F2下断点，按F9执行至此后再按F2取消断点。这个时候EAX寄存器中保存的就是EnumProcesses这个API函数的地址了。该函数的功能是检索进程中的每一个进程标识符(PID)。然后我们在命令栏里输入BP EAX直接给EAX寄存器中的地址下断点。

然后点击上方的B按钮打开断点信息窗口，双击刚刚设置的断点，然后双击反汇编窗口的注释区域来给该函数添加注释,注释上该函数的名称EnumProcesses，OK后然后按减号返回。

继续按F9获取获取的API函数名称，发现下一个API函数是EnumProcessModules。EnumProcessModules的作用是获得指定进程中所有模块的句柄。如果函数执行成功，则返回值为非零。如果函数执行失败，则返回值为零，可以调用 GetLastError函数来获得更多的错误信息。

然后按之前步骤给函数下断点，即先执行到函数返回处，再对EAX寄存器中地址下断点，然后到断点位置添加函数名称注释。

然后继续获取下一个函数，发现下一个待获取的API函数是GetModuleBaseNameA 函数。该函数的作用是获取指定进程模块的名称。同样按之前的步骤给该函数下断点添加注释。

步骤二 分析关闭OllyDbg进程的流程
本步我们跟踪程序来进行流程分析

然后继续按F9，发现程序停在了EnumProcess函数入口76BC3A76地址处，然后我们在函数返回处76BC3B9C地址上按F2下断点，按F9运行至此后再按F2取消断点。

EnumProcess函数此时已经检索完所有进程的PID了，在堆栈窗口点击0012EDE4，在数据窗口定位PID保存位置。

接下来需要找到OllyDbg进程的PID，在最底部任务栏处右击后打开任务管理器，检索OllyDbg的进程ID。查到OllyDbg的进程ID是2680，转化为16进制是A78。注意这里PID的值是不固定的，因此实验时PID会有所区别，只要按照后续的方法是不影响实验的。

然后在数据窗口搜索78 0A（注意倒序搜索），然后选中OllyDbg的PID，设置内存访问断点，按F9运行，发现程序停在访问我们设置断点的内存处。

访问内存断点的是OpenProcess函数。OpenProcess 函数用来打开一个已存在的进程对象，并返回进程的句柄，如成功，返回值为指定进程的句柄，如失败，返回值为空。按F8运行到401D4F处，发现此时的EAX寄存器已经成了88，这就是OllyDbg进程的句柄。

连续按F7单步运行步入401D65处的函数，该函数就是之前设置过断点的EnumProcessModules函数。

我们在函数返回处76BC202B这里按下F2下断点，再按F9后运行至此并按F2取消断点，观察堆栈窗口。栈顶下一行是88（之前获得的OllyDbg进程的句柄），将OD的进程句柄作为EnumProcessModules第一个参数。请求获取进程的模块句柄的时候，系统会返回该模块在进程内存中基地址(模块起始地址)，这里系统返回的是400000，具体实验时可能有所不同。

按F8返回主程序，然后连续按F7单步运行最后步入00401D7E处的函数，这是之前下过断点的GetModuleBaseNameA函数。该函数的作用是获取指定进程模块的名称。按Ctrl+F9运行至函数返回处，观察堆栈窗口，可以看见已经获取了的模块名OLLYDB.EXE，在数据窗口定位到存储模块名OLLYDB.EXE的位置。

继续按F8运行，单步步入401DA1处的函数。一步步调试的过程比较繁琐，我们可以直接在存储模块名OLLYDB.EXE的内存处设置断点，然后直接运行，程序也会中断在访问该地址的代码段处。

按F8单步运行运行进入循环，发现此时ECX寄存器保存着我们获取的模块名的地址，而EDX寄存器保存着“OLLYDBG.EXE”字符串。观察45BAA5到45BAAD处的五行代码，可以解读出这五行代码的意思是将我们获取模块名的第一个字节与EDX中保存的字符串比对。

而这个循环则是将我们获取的模块名与“OLLYDBG.EXE”字符串比对。我们运行到函数返回处。此时EAX寄存器中保存着函数返回值，值为0。

然后我们按F8返回主函数，发现401DA9处的TEST EAX，EAX指令。401DA1处的比对函数，我们可以得出结论：如果进程名不为”OLLYDBG.EXE”，EAX就不等于0，JNZ条件跳转将会发生,如果EAX为0，条件跳转不会发生，将执行后面结束OD进程等一系列操作。

我们按F8继续运行，发现程序运行完401DD5处的TerminateProcess函数后OllyDbg退出。

步骤三 根据流程修改代码绕过反调试检测
本步骤来理清上一步程序关闭进程的流程，并学习如何修改关键指令绕过程序的检测。

程序流程如下所示：

程序运行
GetProcAddress函数获取有关进程操作函数的地址
EnumProcesses函数获取PID
EnumProcessesModules函数获取指定进程中所有模块的句柄
GetModuleBaseNameA函数获取指定进程模块的名称
判断模块名和“OLLYDBG.EXE”是否相同
YES or NO?
YES：OpenProcess函数打开一个已存在进程对象，并返回进程的句柄。程序正常运行
NO：TerminateProcess函数终止指定进程及其所有线程。OllyDbg进程关闭

因此要绕过反调试检测，我们可以修改进行比对之后的跳转指令。

用OllyDbg重新打开程序，查看API函数列表，找到OpenProcess函数，然后下断点，按F9运行，程序中断在OpenProcess函数入口点。

然后按Ctrl+F9运行到函数返回处，单步运行回主程序。接下来查找进行模块名比对后的跳转指令，我们可以看到我们需要找的跳转指令在401DA9处。

我们点击该处指令，按空格键，将JNZ指令改成无条件跳转的JMP指令，移除断点后运行，发现程序已经正常运行。

实验结果分析与总结
我们在进行调试的过程中会遇到很多API函数，在调试之前可以提前学习一些常用的API函数。但是API函数的数量是非常庞大的，我们可以一边进行调试一边学习了解API函数的功能。所以要养成勤查MSDN的好习惯。



