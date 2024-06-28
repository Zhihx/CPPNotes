## 直接初始化direct initialization

## 统一初始化uniform initialization

统一初始化是C++11后出现的特性，其基本用法为用一个```{}```对对象进行初始化（不管是内建类型还是自定义类型，都可以认为创建的是对象）：

```c++
int amount{11}; // 内建类型
```

```c++
#include <iostream>
struct Student
{
	std::string name;
	int age;
	double height;
};
int main()
{
	Student s{"Oliver", 28, 180.0}; // 自定义类型
	std::cout << s.name << std::endl;
}
```