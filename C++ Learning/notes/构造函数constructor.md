## 拷贝构造函数copy constructor

以下代码实现了自定义String类，当未实现拷贝构造函数时，```main```函数中赋值操作只是实现了[[深拷贝与浅拷贝#浅拷贝]]，即两个String对象各自的```m_Buffer```指针指向同一片内存，当调用两个对象的析构函数时，会出现同一片内存释放两次的情况（非指针成员不会出现这个问题）：

```c++
#include <iostream>

class String 
{
public:
	String(const char* string)
	{
		m_Size = strlen(string);
		m_Buffer = new char[m_Size+1];
		memcpy(m_Buffer, string, m_Size * sizeof(char) + 1);
	}
	~String()
	{
		delete[] m_Buffer;
	}
	friend std::ostream& operator<<(std::ostream& stream, const String& string);
private:
	char* m_Buffer;
	unsigned int m_Size;
};

std::ostream& operator<<(std::ostream& stream, const String& string)
{
	stream << string.m_Buffer;
	return stream;
}

int main()
{
	String string = "Cherno";
	String second = string;
	std::cout << string << std::endl;
	std::cout << string << std::endl;
}
```

拷贝构造函数的作用就是实现[[深拷贝与浅拷贝#深拷贝]]，在实现自定义类型对象之间的赋值与拷贝时，实现：
1. 类中非指针成员的值拷贝（具有不同的地址，但是有相同的值）
2. 类中指针成员所指向的内存所存储的值完全相同，但这两个指针所指向的内存地址是不同的（指向不同的内存地址，但指向的内存地址存储的值是相同的）

```c++
#include <iostream>

class String 
{
public:
	String(const char* string)
	{
		m_Size = strlen(string);
		m_Buffer = new char[m_Size+1];
		memcpy(m_Buffer, string, m_Size * sizeof(char) + 1);
	}
	String(const String& other)
	{
		m_Size = other.m_Size;
		m_Buffer = new char[m_Size + 1];
		memcpy(m_Buffer, other.m_Buffer, m_Size * sizeof(char) + 1);
	}
	String& operator=(const String& other)
	{
		m_Size = other.m_Size;
		m_Buffer = new char[m_Size + 1];
		memcpy(m_Buffer, other.m_Buffer, m_Size * sizeof(char) + 1);
	}
	~String()
	{
		delete[] m_Buffer;
	}
	friend std::ostream& operator<<(std::ostream& stream, const String& string);

	char& operator[](size_t index)
	{
		return m_Buffer[index];
	}

	void PrintAddress()
	{
		printf("Address of m_Size: %p\n", &m_Size);
		printf("Address of m_Buffer: %p\n", m_Buffer);
	}
private:
	char* m_Buffer;
	unsigned int m_Size;
};

std::ostream& operator<<(std::ostream& stream, const String& string)
{
	stream << string.m_Buffer;
	return stream;
}

void PrintString(String& string)
{
	std::cout << string << std::endl;
}

int main()
{
	String string = "Cherno";
	String second = string;
	string[2] = 'a';
	PrintString(string);
	PrintString(second);

	printf("------------------------\n");

	string.PrintAddress();
	second.PrintAddress();

	std::cin.get();
}
```

以上代码的运行结果为：
![[copy_constructor.png]]
- 非指针成员的深拷贝：将值拷贝后存放到新分配的内存
- 指针成员的深拷贝：将源指针指向的内存所存储的值拷贝到另一个内存中，并用一个新的指针指向它，这样，两个指针并不会指向同一片内存，也就没有了同一个内存释放两次的问题。
### 禁止拷贝

若要禁止自定义类型对象之间的拷贝，则可以采用：
```c++
String(const String& other) = delete;
String& operator=(const String& other) = delete;
```