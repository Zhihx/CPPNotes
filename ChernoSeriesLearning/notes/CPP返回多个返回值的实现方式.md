#技巧
## 使用结构体
```c++
#include <iostream>
#include <string>
struct Person
{
	std::string Name;
	int age;
};
Person CreatePerson()
{
	return { "Cherno", 18 } ;
}
int main()
{
	Person person = CreatePerson();
	std::cout << person.Name << std::endl;
	std::cin.get();
}
```
## 使用tuple
具体使用方式可以参考[[元组tuple]]
```c++
#include <iostream>
#include <string>
#include <tuple>
std::tuple<std::string, int> CreatePerson()
{
	return { "Cherno", 18 };
}
int main()
{
	auto person = CreatePerson();
	std::string& Name = std::get<0>(person);
	std::cout << Name << std::endl;
	std::cin.get();
}
```
## 使用结构化绑定
[[结构化绑定Structured Bindings]]属于c++17的特性，当函数的返回值存在多种返回值类型的情况时，可以使用
```c++
auto [var1, var2] = FuncReturnMultipleType();
```