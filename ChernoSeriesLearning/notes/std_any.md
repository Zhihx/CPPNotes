#cpp17 

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

"**std: any是一种值类型，它能够更改其类型，同时仍然具有类型安全性**"