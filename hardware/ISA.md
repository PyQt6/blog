
# 指令集体系架构 ISA (未完成编写)


AMD与Intel的指令集

早期的AMD没有任何优势，Intel最早发明指令集，8086处理器发布后，让AMD通过一张图片给“山寨”了同款。
1982年，两家签订授权协议，intel为了防止世界说它搞垄断，将x86指令集授权给了AMD，从此AMD开始了万年老二的代工生涯。
2001年，两家再次签订协议，规定专利可以互用，意思就是技术共享了。AMD发明的3级缓存技术让intel发扬光大了，intel的超线程技术让AMD借鉴了。这些都是为了商业运作，技术从来就不是问题，制造工艺才是瓶颈。

它们的指令集有什么区别？

最基础的指令集功能都是一样的，x86，EM64T，MMX，SSE，SSE2，SSE3。
每一代的cpu会使用不同的针对性改进的指令集，目的就是优化，搞编程的会理解。
AMD最新的指令集SMAP、RDSEED、SHA、XSAVEC、XSAVES、CLFUSHOPT及ADCX等指令集，CLZERO指令集，都是intel没有的，但并不是说它做不出来，代码都是程序员编写，做出来也是大同小异，关键还是看实际效果。比如AMD的推土机CMT指令优化的多核多线程被诟病。

虚拟化、加密等指令集之类的区别比较大；
区别以虚拟化部分为例，内部实现（VT-x <----> AMD-V）基本上是两个概念，所以部分虚拟机可能需要重写一部分加速用的底层代码。谷歌官方安卓模拟器2018年专门发过一个版本里面就干了这个。
对游戏的影响很小，除了部分安卓多开挂机玩家，有很多手游模拟器崩溃都是因为这个。
SIMD指令部分，因为AMD的EPYC不支持原生的AVX512，所以部分浮点算的没有Intel的快。不过这个也不是什么问题：一是你要真较真到底有多少程序能充分利用AVX512的，那确实不多；二是目前大规模并行浮点运算最后总不如优化到显卡上；三是AMD虽然指令集不行，但是核心多啊，在著名分子仿真软件GROMACS测试中，由于Xeon可以用AVX512，双路64核AMD EPYC 7742计算速度仅是双路20核Xeon 6148F的2.7倍。



amd指令集： AMD 3DNow! AMD 3DNow! Professional AMD 3DNowPrefetch AMD Enhanced 3DNow! AMD Extended MMX AMD MisAligned SSE AMD SSE4A AMD SSE5 Cyrix Extended MMX
intel 指令集： IA-32 IA-64 IA MMX IA SSE IA SSE 2 IA SSE 3 IA Supplemental SSE 3 IA SSE 4.1 IA SSE 4.2 IA AVX IA FMA IA AES Extensions
威盛指令集： VIA Alternate Instruction Set CLFLUSH CMPXCHG8B CMPXCHG16B Conditional Move LZCNT MONITOR / MWAIT MOVBE PCLMULQDQ POPCNT RDTSCP SYSCALL / SYSRET SYSENTER / SYSEXIT VIA FEMMS
部分指令集几家cpu厂商共有

Intel的CISC指令集为Intel64,扩展指令集有MMX、SSE、SSE2、SSE3、Sup-SSE3、SSE4.1、SSE4.2、EM64T、VT-x、AVX、AES、VT-d、AVX2、AES-NI、TXT、DBS、TSX等,浮点运算有很大优势
AMD的CISC指令集为AMD64,扩展指令集有3D Now!、3D Now! 、SSE5等,图形处理略有优势.


1、Intel主要有x86，EM64T，MMX，SSE，SSE2，SSE3，SSSE3 (Super SSE3)，SSE4A，SSE4.1，SSE4.2，AVX，AVX2，AVX-512，VMX，AVX 2.0等指令集。
2、AMD主要是x86，x86-64，3D-Now!，MMX（+），3DNOW!（+），SSE，SSE2，SSE3，SSE4A指令集。


## arm

arm的指令集有A64,A32,T32

## 别名

x86=IA-32=i386/i486/i586是Intel提出的指令集
x86-64=amd64是amd提出的指令集，微软称其为x64，其amd的实现称为amd64，其Intel的实现称为Intel64,EM64T,IA-32e
IA-64没有别名，不兼容x86

A64=arm64
A32=旧arm指令集
T32=Thumb32(Thumb2) + Thumb16

## 总结

Intel、AMD的指令集属于CISC指令集，arm的指令集属于RISC指令集

第三代Ryzen处理器采用7nm，支持PCIe4.0



