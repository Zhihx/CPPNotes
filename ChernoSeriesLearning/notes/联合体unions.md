## Level1
<mark style="background: #FFB8EBA6;">union提供了一种使用不同方式解释同一片内存数据的方法</mark>（功能上类似于[[类型双关type punning]]，但是可读性更强），具体来说union经常与struct联用：
```c++
#include <iostream>
struct Vector2
{
	float x, y;
};
struct Vector4
{
	union
	{
		struct
		{
			float x, y, z, w;
		};
		struct
		{
			Vector2 a, b;
		};
	};
};
void PrintVector2(const Vector2& vector)
{
	std::cout << vector.x << " " << vector.y << std::endl;
}
int main() {
	Vector4 vector = { 1.0f, 2.0f, 3.0f, 4.0f };
	std::cout << vector.x << " " << vector.y << std::endl;
	std::cout << vector.z << " " << vector.w << std::endl;
	PrintVector2(vector.a);
	PrintVector2(vector.b);
	std::cin.get();
}
```
结构体Vector4中有一个匿名的union，该union中定义了两种解释内存的方法（即两个匿名结构体），一种是四个float成员变量，另一种是两个Vector2成员
## Level2
union变量所占内存大小是成员所占内存大小的最大值（因为所有成员共享同一片内存）
