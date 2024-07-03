首先是一些直观上的理解：

"streams is the interface for reading and writing data" —— 流即是读写数据的界面
"streams move data from one place to another" —— 流可以移动数据

---
## cin与cout

cin是 std::istream 的一个实例（stream object）
cout 是 std::ostream 的一个实例（stream object）

```>>``` stream extraction operator
```<<``` stream insertion operator

### stream extraction operator

read data one token at a time, rather than one line at a time 
stream extractor operator 一次只读取一个token，而非读取一行
### std::cin的若干注意事项

cin will read up to whitespace
cin会按照whitespace分隔buffer中的内容（token by token）

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

getline函数不会让流处于error状态
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

## stream manipulator

需要include ```iomanip``` 头文件

函数 setw，强迫 ```cout``` 对后续的不足其参数指定的宽度的输出补充正确数量空格（pad its output with the right number of spaces），具体的使用方法为：
```c++
std::cout << setw(5) << 10 << std::endl;
```
函数setw还可以与```std::left```、```std::right```联用，实现左对齐或右对齐，或者与函数setfill联用来指定填充的字符是什么

hex、dec、oct 指定后续输出为十六进制、十进制、八进制

boolalpha 指定后续输出的布尔值返回```true```或```false```

## stream失败

stream失败有可能是因为多种原因，例如文件不存在，或读取的数据类型与指定的数据类型不一致等等。当stream失败时，后续的读写操作将会自动且静默地失败。

成员函数```.fail()```将会判断一个stream object（例如cin或cout）是否处于error状态
成员函数```.clear()```将会取消函数的error状态

有时我们希望从一个文件中不断地读取数据，直到读取失败为止，方法一是：

```c++
std::ifstream input("filename.txt");
while (true)
{
	int value;
	input >> value;
	if (input.fail()) break;
	// do something
}
```

方法二将会更加紧凑：

```c++
std::ifstream input("filename.txt");
int value;
while (input >> value)
{
	// do something
}
```

其基于的原理是，任何的流操作（stream operation）成功时都会返回一个非零值，而失败时则会返回一个零值

## GetInteger函数

该函数的目的主要是为了避免stream extraction operator带来的一些问题

```c++
#include <iostream>
#include <string>
#include <sstream>

std::string GetLine()
{
    std::string str;
    std::getline(std::cin, str);
    if(std::cin.fail())
    {
	    str = "";
	    std::cin.clear();
    }
    return str;
}

int GetInteger()
{
    while(true)
    {
        std::stringstream converter;
        converter << GetLine();
        
        int result;
        if(converter >> result)
        {
            char remaining;
            if(converter >> remaining)
            {
                std::cout << "Unexpected character: " << remaining << std::endl;
            }
            else
            {
                return result;
            }
        }
        else
        {
            std::cout << "Please enter an integer." << std::endl;
        }
        
        std::cout << "Retry: " << std::endl;
    }
}
int main()
{
    GetInteger();
}

```