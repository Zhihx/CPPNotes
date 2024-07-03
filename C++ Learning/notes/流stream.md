
首先是一些直观上的理解：

"streams is the interface for reading and writing data" —— 流即是读写数据的界面
"streams move data from one place to another" —— 流可以移动数据

---
## cin与cout

cin是 std::istream 的一个实例
cout 是 std::ostream 的一个实例

Input streams (I)

● a way to read data from a source  一种从“源”读取数据的方式
	○ Are inherited from ```std::istream```  
	○ ex. reading in something from the console (```std::cin```) 
	○ primary operator: >> (called the extraction operator)

Output streams (O)

● a way to write data to a destination  一种将数据写入“目标”的数据
	○ Are inherited from ```std::ostream``` 
	○ ex. writing out something to the console (```std::cout```) 
	○ primary operator: << (called the insertion operator)

### std::cin的若干注意事项

cin will read up to whitespace
cin会按照whitespace分隔buffer中的内容

Trash in the buffer will make cin not prompt user for input at right time
buffer中错误的内容会导致std::cin不会按照正确的时机读取用户输入

When cin fails, all feature cin operations fail too.
当cin失败时，后续的cin调用也会失败
## stringstream

stringstream 的使用要用include```sstream```头文件

getline函数

```c++
std::istream& getline(std::istream& is, std::string& str, char delim)
```

读取一个```istream```实例的引用，直到遇到分隔符```delim```，将```istream```或buffer中的数据存储到```str```中

getline consumes whitespace（```delim```），具体来说getline在读取buffer中的内容时会将buffer的position pointer移动到whitespace上（注意whitespace并不会作为```str```的内容）
因此，getline的使用需要考虑到buffer的position pointer的位置：

```c++
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::string str1, str2;
    std::istringstream iss("16.9ounces\n 20");
    
    iss >> str1; // 执行后buffer的position pointer指向\n之前 (cin doesn't consume whitespace)
    std::getline(iss, str2); // 执行后position pointer指向\n（getline consumes whitespace）
    
    std::cout << str1 << std::endl;
    std::cout << str2 << std::endl;
    
}
```

因此上述程序的执行结果为
str1: 16.9ounces
str2:(empty，什么都没有，因为whitespace不作为内容输出)

### 什么时候使用stringstream

- Processing string
- Formatting input/output
- Parsing different types

## std::endl与\n的区别

```std::endl```告诉缓存区（intermediate buffer）立马刷新

## fstream

在调用fstream之前需要include ```fstream``` 头文件，一些常用的成员函数有：
- is_open 判断文件是否打开
- open 打开文件
- close 关闭文件

注意fstream会根据数据中的whitespace分隔数据






