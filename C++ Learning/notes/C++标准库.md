## 数据类型

[[C++标准库数据类型]]
## any

```c++
#include <iostream>
#include <any>
#include <string>
int main()
{
	std::any data;
	data = "cherno";
	std::string result = std::any_cast<std::string>(data); // casting
	std::cin.get();
} 
```

"std: any是一种值类型，它能够更改其类型，同时仍然具有类型安全性。也就是说，对象可以保存任意类型的值，但是它们知道当前保存的值是哪种类型。在声明此类型的对象时，不需要指定可能的类型。"
## optional

#cpp17  

```c++
#include <iostream>
#include <fstream>
#include <string>
#include <optional>

std::optional<std::string> ReadFileAsString(const std::string& filepath) // 按常量引用防止拷贝带来的效率降低
{
	std::ifstream stream(filepath);
	if (stream)
	{
		std::string result;
		// read file
		stream.close();
		return result;
	}
	return {}; // 处理“可能为空”的返回值
}
int main()
{
	std::optional<std::string> data = ReadFileAsString("data.txt");
	if (data) // 可以替换为 data.has_value()
	{
		std::cout << "File read sucessfully!\n";
	}
	else
	{
		std::cout << "File could be opened!\n";
	}
	std::cin.get();
}
```

```c++
std::string result = data.value_or("Not presented");
std::cout << result << std::endl;
```

```value_or```指定了当```data```存在特殊情况时的取值

## variant

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