虚析构函数解决的是类继承中子类的析构函数可能会被跳过，只执行父类析构函数，从而导致内存泄漏的问题。

```c++
#include <iostream>
class Base
{
public:
	Base() { std::cout << "Base: Constructor" << std::endl; }
	~Base() { std::cout << "Base: Destructor" << std::endl; }
};
class Derived : public Base
{
public:
	Derived() { std::cout << "Derived: Constructor" << std::endl; }
	~Derived() { std::cout << "Derived: Destructor" << std::endl; }
};
int main() 
{
	Base* base = new Base();
	delete base;

	Derived* derived = new Derived();
	std::cout << "---------------------" << std::endl;
	delete derived;

	std::cin.get();
}
```
代码运行结果为：
![[virtual_destructors_1.png]]

可以看出，类继承中构造函数和析构函数的调用顺序是先执行父类构造函数，然后执行子类构造函数，在调用析构函数时，是先执行子类析构函数，然后执行父类析构函数

当我们在内存中创建一个Derived对象，并使用Base指针指向它时
```c++
Base* derived = new Derived();
```
代码运行结果为：
![[virtual_destructors_2.png]]
可以看出来，此时子类的析构函数被跳过了，这将带来内存泄露问题，如当子类析构函数需要释放子类成员的内存时，因此，需要用[[虚函数virtual functions]]来修饰父类的析构函数，即虚析构函数，来解决这个问题，即
```c++
virtual ~Base() { std::cout << "Base: Destructor" << std::endl; }
```
代码运行结果为：
![[virtual_destructors_3.png]]

**学习虚函数之后的理解**：
虚析构函数是指用```virtual```修饰父类的析构函数，其目的是为了实现子类析构函数的重写，应用场景是当我们用一个父类指针指向子类对象时，子类析构函数被跳过可能会引发内存泄漏问题
