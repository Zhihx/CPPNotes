#cpp17

为了使用结构化绑定，需要确保设置:
Properties-C/C++-Language-C++ Language Standard(ISO C++ 17 Standard)
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

