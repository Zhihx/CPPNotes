
单例模式是指在整个系统生命周期内，保证**一个类只能产生一个实例**，确保该类的唯一性。

```c++
#include <iostream>
class Singleton
{
public:
	Singleton(const Singleton&) = delete; // 禁止拷贝
	static Singleton& Get()
	{
		return s_Instance;
	}
	void print()
	{
		std::cout << m_member << std::endl;
	}
private:
	Singleton() {}
	float m_member = 0.5f;
	static Singleton s_Instance; // 单例模式中唯一的实例，或者说对象
};
Singleton Singleton::s_Instance;
int main()
{
	Singleton& instance = Singleton::Get();
	Singleton::Get().print();
	std::cin.get();
}
```

Line20：这是一个按引用返回类的一个对象的函数，且为static，因此该函数只能调用同样为static的成员变量或成员函数
Line31：注意，这是一个用static修饰的类内成员变量（且该成员变量恰好是该类的一个对象/实例），根据[[关键字Static]]可知，该成员变量被该类的所有对象所有，又因为Get函数按引用返回，因此使用Get函数只能创建一个对象；该行代码可以移动到Get函数中，当运行Get函数则在内存中创建唯一的对象（且Line33可以被删除，见下面的代码段）
Line36：推荐使用引用，因为非引用的方式将导致拷贝，这会带来资源的浪费；Line9的作用是禁止外部拷贝

```c++
#include <iostream>
class Singleton
{
public:
	Singleton(const Singleton&) = delete;
	static Singleton& Get()
	{
		static Singleton s_Instance;
		return s_Instance;
	}
	void print()
	{
		std::cout << m_member << std::endl;
	}
private:
	Singleton() {}
	float m_member = 0.5f;
};
int main()
{
	Singleton& instance = Singleton::Get();
	Singleton::Get().print();
	std::cin.get();
}
```