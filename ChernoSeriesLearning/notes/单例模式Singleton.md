```embed
title: "SINGLETONS in C++"
image: "https://img.youtube.com/vi/PPup1yeU45I/maxresdefault.jpg"
description: "Go to http://www.hostinger.com/cherno and use code “cherno” to get up to 91% OFF yearly web hosting plans. Succeed faster!Patreon ► https://patreon.com/thech…"
url: "https://www.youtube.com/watch?v=PPup1yeU45I&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=82"
```

值得多看几遍，涉及到的知识点有：
- 类，构造函数，拷贝构造函数，公有与私有
- [[静态Static]]
- 命名空间Namespace
---
单例模式是指在整个系统生命周期内，保证**一个类只能 产生一个实例**，确保该类的唯一性。
```c++
#include <iostream>
class Singleton
{
public:
	Singleton(const Singleton&) = delete;
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
	static Singleton s_Instance;
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
Line31：注意，这是一个用static修饰的类内成员变量（且该成员变量恰好是该类的一个对象/实例），根据[[静态Static]]可知，该成员变量被该类的所有对象所有，又因为Get函数按引用返回，因此使用Get函数只能创建一个对象；该行代码可以移动到Get函数中，当运行Get函数则在内存中创建唯一的对象（且Line33可以被删除）
Line36：推荐使用引用，因为非引用的方式将导致浅拷贝，这会带来资源的浪费；Line19的作用是禁止外部拷贝

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