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

默认情况下，结构化绑定返回的是copy，若要返回[[引用reference]]，则需显式地使用引用```&```

结构化绑定也可以适用于[[结构体Struct]]，比如

```c++
#include <iostream>
#include <string>

struct Student
{
    std::string name;
    int age;
    std::string gender;
};

int main()
{
    Student s{"Oliver", 28, "male"};
    
    auto [name, age, gender] = s;
    
    std::cout << "name: " << name << "\nage: " << age << "\ngender: " << gender << std::endl;
    
}

```