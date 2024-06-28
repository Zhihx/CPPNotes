
首先是一些直观上的理解：

"streams is the interface for reading and writing data" —— 流即是读写数据的界面
"streams move data from one place to another" —— 流可以移动数据

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

注意，```std::cin```会根据数据中的whitespace分隔数据，在某些情况下会导致程序无法达成预想中的目标
（具体详见cs106L stream一节最后的例子）
## stringstream

stringstream 的使用要用include```sstream```头文件

getline函数

```c++
std::istream& getline(std::istream& is, std::string& str, char delim)
```

读取一个```istream```的引用，直到遇到分隔符```delim```，将```istream```中的数据存储到```str```中（要注意的是，gteline函数也会返回分隔符）

在[[操作符重载#输出操作符的重载]]中，可以看出都是按引用传递，按引用返回的

## std::endl与\n的区别

```std::endl```告诉缓存区（intermediate buffer）立马刷新

## fstream

在调用fstream之前需要include ```fstream``` 头文件，一些常用的成员函数有：
- is_open 判断文件是否打开
- open 打开文件
- close 关闭文件

注意fstream会根据数据中的whitespace分隔数据






