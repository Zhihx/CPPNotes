### 用类型2解释类型1

```c++
#include <iostream>
int main() {
	int a = 2;
	double value = *(double*)&a;
	std::cout << value << std::endl;
	std::cin.get();
}
```
### 使用指针将结构体数组化

```c++
#include <iostream>
struct Entity
{
	int x, y;
};
int main() {
	Entity e = { 1, 2 };
	int* position = (int*) &e;
	std::cout << position[0] << " " << position[1] << std::endl;
	std::cin.get();
}
```