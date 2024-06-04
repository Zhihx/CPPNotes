- cpp是一种强类型语言
- 实际使用中可能涉及到隐式/显式类型转换（其本质就是我们如何使用另一种方式解释创建的内存）
- 我们更关心的是cpp风格的类型转换
### dynamic_cast

在[[虚函数virtual functions]]中，我们知道虚函数的目的主要是允许父类函数在子类中的重写，当我们使用父类指针指向使用```new```创建的子类对象时，若不使用虚函数，则当使用该父类指针调用成员函数时，将会直接调用父类的同名成员函数，而非子类的成员函数。这是因为，运行父类成员函数还是子类成员函数是通过编译时来进行判断，因此编译器认为调用的是父类的成员函数。而在使用虚函数时，是通过运行时来进行判断，因此调用的是子类的成员函数(因为实际内存创建的是子类对象)

“**dynamic_cast 主要用来在继承体系中的安全向下转型**”
“**安全地将指向基类的指针转型为指向子类的指针或引用，并获知转型动作成功是否**”
“**在执行向下类型转换（基类向子类转换，叫“向下类型转换”），如果expression实际指向的类型与new_type一致，转换才能成功，否则转换失败。**”
“**dynamic_cast的使用需要基类某个成员函数、或构造函数、析构函数标记为virtual**”
“**dynamic_cast的使用最大的问题在于效率”

```c++
#include <iostream>
class Base
{
public:
	virtual void print() { std::cout << "Base" << std::endl; }
};
class Derived : public Base
{
public:
	void print() { std::cout << "Derived" << std::endl; }
};
int main()
{
	Base* pBase = new Derived();
	Derived* pDerived = dynamic_cast<Derived*>(pBase);
	pBase->print();
	pDerived->print();
	std::cin.get();
}
```
 