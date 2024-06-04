如何评价一段cpp程序的性能
<mark style="background: #FFB8EBA6;">自定义Timer类(这是一个作用域定时器)</mark>
```c++
#include <iostream>
#include <memory>
#include <chrono>
class Timer
{
public:
	Timer() 
	{
		m_StartTimePoint = std::chrono::high_resolution_clock::now();
	}
	~Timer()
	{
		Stop();
	}
	void Stop()
	{
		auto endTimePoint = std::chrono::high_resolution_clock::now();
		auto start = std::chrono::time_point_cast<std::chrono::microseconds>(m_StartTimePoint).time_since_epoch().count();
		auto end = std::chrono::time_point_cast<std::chrono::microseconds>(endTimePoint).time_since_epoch().count();

		auto duration = end - start;
		double ms = duration * 0.001;

		std::cout << duration << "us (" << ms << "ms)\n";
	}
private:
	std::chrono::time_point<std::chrono::high_resolution_clock> m_StartTimePoint;
};

int main()
{
	int value = 0;
	{
		Timer timer;
		for (int i = 0; i < 20000; i++)
			value += 2;
	}
	std::cout << value << std::endl;
	std::cin.get();
}
```
将Timer类对象与想要评价的一段代码放入一个作用域中，当作用域结束，将自动调用Timer类对象的析构函数，并计算时间

- Timer类的构造函数会在构建Timer对象时，获得当前时间点
- Timer类的析构函数会在销毁Timer对象时，调用Stop函数
- ```high_resolution_clock::now``` 返回当前时间点
- ```std::chrono::time_point_cast<std::chrono::microseconds>(m_StartTimePoint).time_since_epoch().count()```将```m_StartTimePoint```转换为微秒数据