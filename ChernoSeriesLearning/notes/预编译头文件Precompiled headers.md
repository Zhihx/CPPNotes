<mark style="background: #FFB8EBA6;">预编译头文件（Precompiled Header，PCH）是一种可以用来提高编译速度的技术。它的原理是先将一些常用的头文件预处理，生成一个二进制文件（通常是.pch文件），然后在编译其他源文件时，直接引用这个二进制文件，避免了重复的预处理过程，从而提高了编译速度。</mark>
(https://www.cnblogs.com/yangjj08/p/17463468.html)

“**不要把频繁修改的代码放进预编译头文件中**”

在visual studio中创建预编译头的方法为： 

首先，<mark style="background: #FFB8EBA6;">创建一个.h文件</mark>，以及一个<mark style="background: #FFB8EBA6;">include该头文件的同名.cpp文件</mark>，将需要预编译的所有代码放入该.h文件中，例如：
pch.h文件内容为：
```c++
#include <iostream>
```
pch.cpp文件内容为：
```c++
#include "pch.h"
```

右击pch.h文件，选择<mark style="background: #FFB8EBA6;">Properties-Precompiled Headers-Precompiled Headers(create(/Yc))</mark>

右键单击整个项目，选择<mark style="background: #FFB8EBA6;">Properties-Precompiled Headers-Precompiled Headers(use(/Yu))
Precompiled Header File</mark>选择为pch.h

为了查看编译时间，选择<mark style="background: #FFB8EBA6;">Tools-Options-Projects and Solutions-VC++ Project Settings-Build Timing(Yes)</mark>

会发现使用预编译头文件和不使用预编译头文件的时间分别为825ms和1267ms