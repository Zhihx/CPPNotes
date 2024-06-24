## iterator在for-loop中的基本使用

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <string>
int main()
{
	std::vector<int> values = { 1, 2, 3, 4, 5 };
	// for-loop
	for (auto i : values)
	{
		std::cout << i << std::endl;
	}
	// iterator for std::vector
	for (std::vector<int>::iterator it = values.begin(); 
		it != values.end(); it++)
	{
		std::cout << *it << std::endl;
	}
	
	std::unordered_map<std::string, int> map;
	map["Cherno"] = 5;
	map["C++"] = 10;
	// iterator for std::unordered_map
	for (std::unordered_map<std::string, int>::const_iterator it = map.begin();
		it != map.end(); it++)
	{
		auto& key = it->first;
		auto& value = it->second;
		std::cout << key << " = " << value << std::endl;
	}
	// structured bindings
	for (auto [key, value] : map)
	{
		std::cout << key << " = " << value << std::endl;
	}
	std::cin.get();
} 
```

Line33：使用[[结构化绑定Structured Bindings]]与range-for-loop也可以实现简洁地遍历某种集合数据类型的元素遍历（注意要将C++语言标准调至17）
## 自定义iterator用于自定义数据结构

vector.h

```c++
#pragma once // 避免该头文件被多次include

template<typename Vector> // 用typename指定一个类型为Vector的参数
class VectorIterator
{
public:
	using ValueType = typename Vector::ValueType; // 用using指定类型Vector::ValueType的别名
	using ReferenceType = ValueType&; // 用using指定类型ValueType&的别名
	using PointerType = ValueType*; // 用using指定类型ValueType*的别名
public:
	VectorIterator(PointerType ptr) // 一般构造函数，使用参数列表实现参数的初始化
		: m_Ptr(ptr) {}

	VectorIterator& operator++() // 前缀自增运算符的重载
	{
		m_Ptr++;
		return *this; // 对指向当前对象的指针this做dereference
	}
	VectorIterator& operator++(int) // 后缀自增运算符的重载，需要在参数列表中指定int
	{
		VectorIterator iterator = *this;
		++(*this);
		return iterator; // 返回该类型的引用，避免拷贝带来的效率的降低
	}
	VectorIterator& operator--() // 前缀自增运算符的重载
	{
		m_Ptr--;
		return *this;
	}
	VectorIterator& operator--(int) // 后缀自增运算符的重载，需要在参数列表中指定int
	{
		VectorIterator iterator = *this;
		--(*this);
		return iterator; // 返回该类型的引用，避免拷贝带来的效率的降低
	}
	ReferenceType operator[](size_t index) // 索引操作符的重载
	{
		return *(m_Ptr + index);
	}
	PointerType operator->() // 箭头操作符的重载
	{
		return m_Ptr;
	}
	ReferenceType operator*() // 对指针的dereference
	{
		return *m_Ptr;
	}
	bool operator==(const VectorIterator& other) const // 逻辑运算符的重载
	{
		return m_Ptr == other.m_Ptr;
	}
	bool operator!=(const VectorIterator& other) const // 逻辑运算符的重载
	{
		return m_Ptr != other.m_Ptr;
	}
private:
	PointerType m_Ptr;
};


template<typename T>
class Vector
{
public:
	using ValueType = T; // 定义类型别名，VectorIterator类如果要调用则需要使用对Vector使用域操作符
	using Iterator = VectorIterator<Vector<T>>; // 定义类型别名

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

	VectorIterator<Vector<ValueType>> begin()
	{
		return VectorIterator<Vector<ValueType>>(m_Data);
	}

	VectorIterator<Vector<ValueType>> end()
	{
		return VectorIterator<Vector<ValueType>>(m_Data + m_Size);
	}

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

```c++
#include <iostream>
#include "Vector.h"

int main()
{
	Vector<int> values;
	values.EmbraceBack(2.0);
	values.EmbraceBack(3.0);
	values.EmbraceBack(4.0);
	values.EmbraceBack(5.0);
	values.EmbraceBack(6.0);
	values.EmbraceBack(7.0);

	for (size_t i = 0; i < values.Size(); i++)
	{
		std::cout << values[i] << std::endl;
	}

	for (VectorIterator<Vector<int>> it = values.begin(); // 自定义的iterator
		it != values.end(); it++)
	{
		std::cout << *it << std::endl;
	}

	std::cin.get();
}
```