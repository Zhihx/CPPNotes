如何追踪C++在哪里分配内存，以及追踪应用程序的内存分配情况

```c++
#include <iostream>
#include <string>

void* operator new(size_t size) // 关键在于重载new表达式
{
	std::cout << "Allocating " << size << " bytes\n" << std::endl;
	return malloc(size);
}

void operator delete(void* memory, size_t size)
{
	std::cout << "Freeing " << size << " bytes\n" << std::endl;
	free(memory);
}

int main()
{
	{
		std::string str = "cherno"; 
	}
	std::cin.get();
}
```

Line7&Line13：```size_t```的真实类型与操作系统有关，其作为```unsigned```的```typedef```，其用来表示程序运行中所有的内存地址；
Line7&Line13：运算符重载

```c++
#include <iostream>
#include <string>

struct AllocationMetrics
{
	uint32_t TotalAllocated = 0;
	uint32_t TotalFreed = 0;

	uint32_t CurrentUsage() { return TotalAllocated - TotalFreed; }
};

static AllocationMetrics s_AllocationMetrics;

void* operator new(size_t size)
{
	s_AllocationMetrics.TotalAllocated += size;
	return malloc(size);
}

void operator delete(void* memory, size_t size)
{
	s_AllocationMetrics.TotalFreed += size;
	free(memory);
}

static void PrintMemoryUsage()
{
	std::cout << "Memory Usage: " << s_AllocationMetrics.CurrentUsage() << " bytes\n" << std::endl;
}

int main()
{
	PrintMemoryUsage();
	{
		std::string str = "cherno"; 
		PrintMemoryUsage();
	}
	PrintMemoryUsage();
	std::cin.get();
}
```

Line43：定义了一个用```static```修饰的结构体变量，该变量在整个程序生命周期都将存在
Line57：定义了一个用```static```修饰的函数，该函数只在本源码文件中可见
