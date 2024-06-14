首先先明确一下堆、栈上定义数组的不同点：
栈上定义数组的方式为：
```c++
int array[5];
```
堆上定义数组的方式为：
```c++
int size = 5;
int* heapArray = new int[size];
delete[] heapArray;
```
栈上定义数组时，数组的长度必须在编译时就已经确定，因此必须使用字面量或者是被```const```修饰的不可修改的变量。（此处有一个需要学习的隐藏知识点，即如何确定什么东西在编译时就可以确定）

C++还提供了一种标准数组类，其使用方法为：
```c++
#include <array>
std::array<int, 10> collection;
for (int i : collection)
{
	std::cout << collection[i] << std::endl;
}
```
---
如何使用上述知识点实现简单的自定义Array类，该类可以满足的功能有：
- 自定义Array中元素的数据类型，Array元素个数（通过使用[[模版template]]），并返回元素个数
- 初始化Array中的所有元素
- 通过索引访问Array中的元素（通过[[运算符重载]]），并判断索引的有效性（通过[[断言assert#静态断言]]）

```c++
#include <iostream>

template <typename T, size_t S> // 模版的使用，可以自定义Array中元素的数据类型，元素个数
class Array
{
public:
	constexpr int Size() const { return S; } // const表明函数不会修改该类成员，constexpr表明这是常量表达式
	T& operator[] (size_t index) // 重载索引表达式，注意按引用返回，一避免无意义的拷贝，而作为左值可被修改
	{ 
		if (!(index < S))
		{
			__debugbreak(); // 当索引值不合理时，编译器break
		}
		return m_Data[index]; 
	}
	const T& operator[] (size_t index) const { return m_Data[index]; } // 同名函数，用于不修改的情形
	T* Data() { return m_Data; }
	const T* Data() const { return m_Data; }
private:
	T m_Data[S];
};

int main()
{
	Array<int, 5> data;

	memset(data.Data(), 0, data.Size() * sizeof(int)); // 初始化

	for (int i = 0; i < data.Size(); i++)
	{
		// data[i] = 2;
		std::cout << data[i] << std::endl;
	}
	std::cin.get();
} 
```