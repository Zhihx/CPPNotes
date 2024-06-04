#cpp17 
```std::variant```和[[联合体unions]]类似，其目的都是为了使用多种数据类型。当一种数据类型被使用时，其他数据类型将不被定义。
```c++
#include <iostream>
#include <variant>

int main()
{
	std::variant<std::string, int> data;
	data = "cherno";
	std::cout << std::get<std::string>(data) << std::endl;
	data = 2;
	std::cout << std::get<int>(data) << std::endl;;
	std::cin.get();
}
```

```std::get<classType1>(data)```在给定数据类型或给定索引的情况下，返回variant的值