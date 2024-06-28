
按引用传递
“Hey take in the actual piece of memory, don’t make a copy”

```c++
#include <iostream>
#include <vector>
void increment(std::vector<std::pair<int, int>> &nums)
{
    for(auto& [num1, num2] : nums) // 结构化绑定也需要引用???
    {
        num1++;
        num2++;
    }
}
int main()
{
    std::vector<std::pair<int, int>> nums = {{1,1}, {2,2}, {3,3}};
    for(auto [num1, num2] : nums)
    {
        std::cout << num1 << " " << num2 << std::endl;
    }
    increment(nums);
    for(auto [num1, num2] : nums)
    {
        std::cout << num1 << " " << num2 << std::endl;
    }
}
```

函数```increment```中for-loop中的[[结构化绑定Structured Bindings]]也需要采取引用的方式才能实现修改输入的vector，猜测结构化绑定也类似于一个函数，```auto&```按引用返回才能实现对内存的修改

注意：
1. 对声明为常量的变量不能使用非常量引用，编译器会报错，除非使用常量引用