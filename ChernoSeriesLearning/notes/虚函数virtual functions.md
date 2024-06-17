- <mark style="background: #FFB8EBA6;">通过将父类方法修饰为虚函数，可以在子类中重写该方法实现其他功能</mark>
- <mark style="background: #FFB8EBA6;">虚函数是一种实现多态的方式</mark>
- <mark style="background: #FFB8EBA6;">具体来说，cpp通过存储vtable虚表，并查找虚表的方式实现多态（牺牲了内存和查找虚表所需要的时间）</mark>

```c++
#include <iostream>
#include <string>
class Base
{
public:
	std::string GetName() { return "Base"; }
};
class Derived : public Base
{
private:
	std::string m_name;
public:
	Derived(const std::string& name) 
		: m_name(name) {}
	std::string GetName() { return m_name; }
};
int main() 
{
	Base* base = new Base();
	std::cout << base->GetName() << std::endl;

	Derived* derived = new Derived("Derived");
	std::cout << derived->GetName() << std::endl;

	std::cin.get();
}
```
上述程序中，子类重写了父类的函数```GetName()```，很明显程序运行的结果为：
![[virtual_func_1.png]]
然而上述程序并不满足多态性的要求，例如，当我们在内存中创建一个```Derived```对象，并使用一个```Base```指针指向它时：
```c++
Base* derived = new Derived("Derived");
```
将出现错误：
![[virtual_func_2.png]]
这是因为，当使用```Base```指针指向一个对象并调用相应函数时，cpp默认在```Base```类中寻找可以使用的函数，从而忽略了子类可能重写该函数，为了解决这个问题：
```c++
#include <iostream>
#include <string>
class Base
{
public:
	virtual std::string GetName() { return "Base"; }
};
class Derived : public Base
{
private:
	std::string m_name;
public:
	Derived(const std::string& name) 
		: m_name(name) {}
	std::string GetName() override { return m_name; } // 使用override可以增加可读性
};
int main() 
{
	Base* base = new Base();
	std::cout << base->GetName() << std::endl;

	Base* derived = new Derived("Derived");
	std::cout << derived->GetName() << std::endl;

	std::cin.get();
}
```
程序的运行结果为：
![[virtual_func_3.png]]