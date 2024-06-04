#cpp17  
```c++
#include <iostream>
#include <fstream>
#include <string>
#include <optional>
std::optional<std::string> ReadFileAsString(const std::string& filepath)
{
	std::ifstream stream(filepath);
	if (stream)
	{
		std::string result;
		// read file
		stream.close();
		return result;
	}
	return {};
}
int main()
{
	std::optional<std::string> data = ReadFileAsString("data.txt");
	if (data)
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
- ```std::optional```的目的是为了设想一种情况，以定义某变量可能存在的特殊情况。如上述代码则是假定了文件不能打开的情况，则返回```{}```(空选项 in LINE 17)
- LINE 22 可以替换为```data.has_value()```

```c++
std::string result = data.value_or("Not presented");
std::cout << result << std::endl;
```

```value_or```指定了当```data```存在特殊情况时的取值