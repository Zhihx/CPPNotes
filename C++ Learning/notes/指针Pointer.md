“所有类型的指针(包括指针的指针的指针……)都是一个整数(integer)，其存储一个内存地址”
“我们指定指针的类型只是为了告诉计算机，这个内存地址的数据可能是什么类型”
“```void```类型的指针意味着我们不关心这个内存地址的数据是什么类型的”
## 原始指针

1. ```0```是无效的内存地址，其等价为```NULL```、```nullptr```
2. 可以通过取地址符```&```来获得一个变量的地址
3. 可以通过```*```访问指针指向的那个内存地址所存储的数据
4. 类型的重要性体现在我们想要读取指针所指向的那个内存地址所存储的数据
5. 定义一个存储堆上内存地址的指针的方法为：

```c++
char* buffer = new char[8];
memset(buffer, 0, 8 * sizeof(char))
```

6. 指针的指针（本质上还是一个整数，存放的是指针本身的内存）

```c++
int var = 10;
int* ptr1 = &var;
int** ptr2 = &ptr1;
std::cout << **ptr2 << std::endl;
```
## 智能指针
“智能指针是在```<memory>```头文件中的```std```命名空间中定义的”
“智能指针是对原始指针的一种封装，它会自动调用```new```申请内存，然后基于使用的智能指针的具体类型，在某一时刻自动释放分配的内存”
### ```unique_ptr```
- ```unique_ptr```是一个作用域指针，当超出作用域后，指针将自动销毁
- ```unique_ptr```无法复制到其他的```unique_ptr```，这是因为在```unique_ptr```类中的拷贝构造函数以及重载赋值操作都被禁止了:
```c++
unique_ptr(const unique_ptr&)            = delete;
unique_ptr& operator=(const unique_ptr&) = delete;
```
- ```unique_ptr```的使用方法为：
```c++
#include <iostream>
#include <memory>
class Entity
{
public:
	Entity() { std::cout << "Created Entity" << std::endl; }
	~Entity() { std::cout << "Destroyed Entity" << std::endl; }
	void Print() {}
};
int main()
{
	{
		std::unique_ptr<Entity> entity = std::make_unique<Entity>(); // 异常安全
		entity->Print();
	}
}
```
### ```shared_ptr```
“```shared_ptr```使用引用计数，每一个```shared_ptr```的拷贝都指向相同的内存。每使用他一次，内部的引用计数加1，每析构一次，内部的引用计数减1，减为0时，删除所指向的堆内存。```shared_ptr```内部的引用计数是安全的。”
### ```weak_ptr```
当将一个```shared_ptr```对象赋值给一个```weak_ptr```对象时，不会增加引用计数