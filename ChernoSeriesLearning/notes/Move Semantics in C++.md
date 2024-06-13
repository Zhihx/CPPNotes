C++中常见构造函数的总结
- 默认构造函数
- 普通构造函数
- 拷贝构造函数
- 转移构造函数
- <mark style="background: #FFB8EBA6;">移动构造函数</mark>

移动语义是在[[左值与右值lvalues and rvalues]]的基础上产生的
## 移动构造函数

**移动构造是C++11标准中提供的一种新的构造方法。在C++11之前，如果要将源对象的“状态”转移到目标对象只能通过复制，或者说深拷贝，即使用拷贝构造函数**

拷贝构造函数实现临时对象的拷贝原理为：临时对象申请堆内存，目标对象申请堆内存，通过拷贝构造函数将临时对象的“状态”赋值给目标对象

移动构造函数实现临时对象的移动原理为：临时对象申请堆内存，该堆内存由目标对象接管，因此相比于拷贝构造函数而言，少了一份目标对象申请的堆内存

```c++
#include <iostream>

class String
{
public:
	String() = default;
	String(const char* string) 
	{
		printf("Created\n");
		m_Size = strlen(string);
		m_Data = new char[m_Size];
		memcpy(m_Data, string, m_Size);
	}
	String(const String& other) // 拷贝构造函数的参数是常量左值引用
	{
		printf("Copied\n");
		m_Size = other.m_Size;
		m_Data = new char[m_Size];
		memcpy(m_Data, other.m_Data, m_Size);
	}
	String(String&& other) noexcept // 移动构造函数的参数是右值引用
	{
		printf("Moved\n");
		m_Size = other.m_Size;
		m_Data = other.m_Data;
		other.m_Size = 0; //
		other.m_Data = nullptr; // 需要将临时对象中的指针设置为nullptr
	}
	~String()
	{
		printf("Destroyed\n");
		delete m_Data;
	}
	void print()
	{
		for (uint32_t i = 0; i < m_Size; i++)
		{
			printf("%c", m_Data[i]);
		}
		printf("\n");
	}
private:
	char* m_Data;
	uint32_t m_Size;
};

class Entity
{
public:
	Entity(const String& name)
		: m_Name(name)
	{}
	Entity(String&& name)
		: m_Name((String&&) name) // 为了保证调用的是移动构造函数，这里也需要使用右值引用
	{}
	void printName()
	{
		m_Name.print();
	}
private:
	String m_Name;
};

int main()
{
	Entity entity(String("cherno")); // 临时对象
	entity.printName();
	std::cin.get();
} 
```

程序的运行结果为：
![[move_1.png]]
## std::move
```std::move```的作用是将现有变量转换为临时变量，其等价于对现有变量(左值)做右值引用的casting，将其转换为右值
```c++
#include <iostream>

class String
{
public:
	String() = default;
	String(const char* string) 
	{
		printf("Created\n");
		m_Size = strlen(string);
		m_Data = new char[m_Size];
		memcpy(m_Data, string, m_Size);
	}
	String(const String& other)
	{
		printf("Copied\n");
		m_Size = other.m_Size;
		m_Data = new char[m_Size];
		memcpy(m_Data, other.m_Data, m_Size);
	}
	String(String&& other) noexcept
	{
		printf("Moved\n");
		m_Size = other.m_Size;
		m_Data = other.m_Data;
		other.m_Size = 0;
		other.m_Data = nullptr;
	}
	String& operator=(String&& other) noexcept // 增加了一个对赋值号的重载
	{
		printf("Moved\n");
		if (this != &other) // 避免出现对同一对象进行赋值的情况
		{
			delete[] m_Data; // 因为当前对象中的成员指针指向临时对象的成员指针所指向的内存，则之前指向的内存必须被释放，否则会出现内存泄漏的问题
			m_Size = other.m_Size;
			m_Data = other.m_Data;
			other.m_Size = 0;
			other.m_Data = nullptr;
		}
		return *this; // 如果是对同一对象赋值，则返回当前对象
	}
	~String()
	{
		printf("Destroyed\n");
		delete[] m_Data;
	}
	void print()
	{
		for (uint32_t i = 0; i < m_Size; i++)
		{
			printf("%c", m_Data[i]);
		}
		printf("\n");
	}
private:
	char* m_Data;
	uint32_t m_Size;
};

class Entity
{
public:
	Entity(const String& name)
		: m_Name(name)
	{}
	Entity(String&& name)
		: m_Name((String&&)name)
	{}
	void printName()
	{
		m_Name.print();
	}
private:
	String m_Name;
};

int main()
{
	String string = "Hello";
	// String dest(std::move(string));
	// String dest = std::move(string);
	String dest;
	dest = std::move(string);
	std::cin.get();
} 
```