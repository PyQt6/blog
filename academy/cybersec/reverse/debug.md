
# 调式 debug

## 知识梳理

反调试：为了防止程序被破解，增加破解人员分析的难度，很多程序在开发时会加入反调试技术，被调试的程序可以检测到自己是否被调试，如果被调试，则程序可能改变正常的执行路径或者修改自身程序让自己崩溃，亦或者直接退出程序，通常反调试技术也被各种恶意软件所运用，大大延长恶意代码分析的时间。  
IsDebuggerPresent()：IsDebuggerPresent()函数是最简单最基础的一种反调试技术，这是Windows提供的一个API接口，通过访问PEB的BeingDebugged标志来判断是否处于调试状态，如果处于调试状态则返回1。   
atoi()：表示ascii to integer，是把字符串转换成整型数的一个函数  
本步骤通过静态分析程序，可以大致了解程序的流程是怎样的，做了哪些操作，对输入做了怎样的check。对程序有个大致的概念，方便我们继续深入的分析以及动态调试。



