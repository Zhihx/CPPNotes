## How to make strings faster

C++生成处理字符串，会不断地分配内存，这将降低程序运行速度，解决方法为：
```const char*``` 与 ```std::string_view```的联用

```const char*```是C语言风格的字符串

首先是只使用```std::string```的情况
```c++
#include <iostream>
#include <string>

static uint32_t s_AllocCount = 0;

void* operator new(size_t size)
{
	s_AllocCount++;
	std::cout << "Allocating " << size << " bytes\n";
	return malloc(size);
}

void PrintName(const std::string& name)
{
	std::cout << name << std::endl;
}

int main()
{
	std::string name = "Zhihan XU";
	std::string firstname = name.substr(0, 6);
	std::string lastname = name.substr(7, 2);

	PrintName(name);
	PrintName(firstname);
	PrintName(lastname);
}
```
程序运行的结果为：
![[faster_string_1.png]]
可以看出，仅仅只是创建字符串，提取字符串(2次)，就涉及到了多次的内存分配，这将极大地拖慢程序运行的速度。

其次是使用```std::string_view```的情况
```c++
#include <iostream>
#include <string>

static uint32_t s_AllocCount = 0;

void* operator new(size_t size)
{
	s_AllocCount++;
	std::cout << "Allocating " << size << " bytes\n";
	return malloc(size);
}

void PrintName(const std::string_view name)
{
	std::cout << name << std::endl;
}

int main()
{
	std::string name = "Zhihan XU";
	std::string_view firstname(name.c_str(), 6);
	std::string_view lastname(name.c_str()+7, 2);

	PrintName(name);
	PrintName(firstname);
	PrintName(lastname);
}
```
程序的运行结果为：
![[faster_string_2.png]]
可以看出来，此时只涉及到一次内存的分配(创建```std::string```)
注意，```c_str()```返回当前字符串首字符地址

最后是```const char*``` 与 ```std::string_view```的联用
```c++
#include <iostream>
#include <string>

static uint32_t s_AllocCount = 0;

void* operator new(size_t size)
{
	s_AllocCount++;
	std::cout << "Allocating " << size << " bytes\n";
	return malloc(size);
}

void PrintName(const std::string_view name)
{
	std::cout << name << std::endl;
}


int main()
{
	const char* name = "Zhihan XU";
	std::string_view firstname(name, 6);
	std::string_view lastname(name+7, 2);

	PrintName(name);
	PrintName(firstname);
	PrintName(lastname);

	std::cin.get();
}
```
程序运行结果为：
![[faster_string_3.png]]
在使用C语言风格的字符串```const char*```后，整个程序不涉及到字符串的内存分配问题
## Small string optimization

用于低于一定长度的字符串来避免过多的内存分配和堆分配(heap allocation)
小字符串将存储在静态存储区(static buffer)而非堆中

注意：为了让小字符串存储在静态存储区中，在VS中可能需要使用release模式