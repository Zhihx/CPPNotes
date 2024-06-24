
栈上定义数组的方式为：

```c++
int array[5];
int array[] = {1, 2, 3, 4, 5}; // 通过初始化的元素个数自动确定数组的大小

const unsigned size = 5; // 指定size时需要确保编译器在编译时可以确定
constexpr int size = 5; // 因此可以使用字面量、const、constexpr
int array[size];
```

堆上定义数组的方式为：

```c++
int size = 5;
int* heapArray = new int[size];
delete[] heapArray;
```

栈上定义数组时，数组的长度必须在编译时就已经确定，因此必须使用字面量或者是被```const```、```constexpr```修饰的不可修改的变量。相反，堆上确定的数组长度可以不需要再编译时确定

C++还提供了一种标准数组类，该标准数组类的特点是长度不能像[[C++标准库数据类型#Vector]]一样可以灵活变化，其使用方法为：

```c++
#include <array>
std::array<int, 10> collection;
for (int i : collection) // range for-loop
{
	std::cout << collection[i] << std::endl;
}
```
## 自定义Array（栈）

自定义Array类，该类可以满足的功能有：
- 自定义Array中元素的数据类型，Array元素个数（通过使用[[模板template]]），并返回元素个数
- 初始化Array中的所有元素
- 通过索引访问Array中的元素（通过[[操作符重载]]），并判断索引的有效性（通过[[断言assert#静态断言]]）

```c++
#include <iostream>

template <typename T, size_t S> // 模板参数，T是一个数据类型，S是一个size_t类型的变量，用于指定该Array的长度
class Array
{
public:
	constexpr int Size() const { return S; } // const表明函数不会修改该类成员，constexpr表明这是常量表达式
	T& operator[] (size_t index) // 重载索引表达式，注意按引用返回
	{ 
		if (!(index < S))
		{
			__debugbreak(); // 当索引值不合理时，编译器break
		}
		return m_Data[index]; 
	}
	const T& operator[](size_t index) const { return m_Data[index]; } // 同名函数，用于不修改的情形
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
		data[i] = 2;
		std::cout << data[i] << std::endl;
	}
	std::cin.get();
} 
```
## 自定义Vector

vector.h

```c++
#pragma once

template<typename T>
class Vector
{
public:
	Vector()
	{
		// allocate 2 elements
		ReAlloc(2);
	}

	~Vector()
	{
		delete[] m_Data;
	}

	void PushBack(const T& value)
	{
		if (m_Size >= m_Capacity)
			ReAlloc(m_Capacity + m_Capacity / 2);
		m_Data[m_Size] = value;
		m_Size++;
	}
	void PushBack(T&& value)
	{
		if (m_Size >= m_Capacity)
			ReAlloc(m_Capacity + m_Capacity / 2);
		m_Data[m_Size] = std::move(value);
		m_Size++;
	}

	template<typename... Args>
	T& EmbraceBack(Args&&... args)
	{
		if (m_Size >= m_Capacity)
			ReAlloc(m_Capacity + m_Capacity / 2);
			m_Data[m_Size] = T(std::forward<Args>(args)...);
		return m_Data[m_Size++];
	}

	void PopBack()
	{
		if (m_Size > 0)
		{
			m_Size--;
			m_Data[m_Size].~T();
		}
	}

	void Clear()
	{
		for (size_t i = 0; i < m_Size; i++)
		{
			m_Data[i].~T();
		}
		m_Size = 0;
	}

	T& operator[](size_t index) 
	{ 
		return m_Data[index]; 
	}
	
	const T& operator[](size_t index) const 
	{ 
		return m_Data[index]; 
	}

	size_t Size() const { return m_Size; }

private:
	void ReAlloc(size_t newCapacity)
	{
		// 1. allocate a new block of memory
		// 2. copy/move old elements into new block
		// 3. delete
		T* newBlock = new T[newCapacity];

		if (newCapacity < m_Size)
			m_Size = newCapacity;

		for (size_t i = 0; i < m_Size; i++)
		{
			newBlock[i] = std::move(m_Data[i]);
		}
		delete[] m_Data;
		m_Data = newBlock;
		m_Capacity = newCapacity;
	}
private:
	T* m_Data = nullptr;

	size_t m_Size = 0;
	size_t m_Capacity = 0;
};
```

main.cpp

```c++
#include <iostream>

#include <string>
#include "Vector.h"


struct Vector3
{
	float x = 0.0f, y = 0.0f, z = 0.0f;

	Vector3() = default;
	Vector3(float scalar)
		: x(scalar), y(scalar), z(scalar) {}
	Vector3(float x, float y, float z)
		: x(x), y(y), z(z) {}
	Vector3(const Vector3& other)
	{
		std::cout << "Copied!\n";
		x = other.x;
		y = other.y;
		z = other.z;
	}
	Vector3(Vector3&& other) noexcept
	{
		std::cout << "Moved!\n";
		x = other.x;
		y = other.y;
		z = other.z;
	}
	Vector3& operator=(const Vector3& other)
	{
		std::cout << "Copied!\n";
		x = other.x;
		y = other.y;
		z = other.z;
		return *this;
	}
	Vector3& operator=(Vector3&& other) noexcept
	{
		std::cout << "Moved!\n";
		x = other.x;
		y = other.y;
		z = other.z;
		return *this;
	}

	~Vector3()
	{
		std::cout << "Destroyed!\n";
	}
};

void PrintVector(const Vector<Vector3>& vector)
{
	for (size_t i = 0; i < vector.Size(); i++)
	{
		std::cout << vector[i].x << ", " << vector[i].y << ", " << vector[i].z << std::endl;
	}
	std::cout << "----------------------------------" << std::endl;
}

int main()
{
	Vector<Vector3> vector;
	vector.PushBack(Vector3(1.0, 2.0, 3.0));
	vector.PushBack(Vector3(1.0, 2.0, 3.0));
	vector.EmbraceBack(2.0, 3.0, 4.0);
	PrintVector(vector);
	vector.PopBack();
	PrintVector(vector);
	vector.Clear();
	std::cin.get();
} 
```