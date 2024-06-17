#cpp17

为了使用结构化绑定，需要确保设置:
<mark style="background: #FFB8EBA6;">Properties-C/C++-Language-C++ Language Standard(ISO C++ 17 Standard)</mark>
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
	auto [name, age] = CreatePerson();
	std::cout << name << std::endl;
	std::cin.get();
}
```

