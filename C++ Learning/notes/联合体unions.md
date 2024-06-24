
union提供了**一种使用不同方式解释同一片内存数据的方法**（功能上类似于[[类型双关type punning]]，但是可读性更强），具体来说union经常与struct联用：

```c++
#include <iostream>
struct Vector2
{
	float x, y;
};
struct Vector4
{
	union // 匿名联合体
	{
		struct // 匿名结构体
		{
			float x, y, z, w;
		};
		struct // 匿名结构体
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
## 注意

union变量所占内存大小是成员所占内存大小的最大值（因为所有成员共享同一片内存）
