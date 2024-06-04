#cpp11
## 头文件
```c++
#include <tuple>
```
## 构建方式
```c++
std::tuple<classType1, classType2> t1;
std::tuple<classType1, classType2> t2(val1, val2);
```
第二种构建方式将直接对元组中的元素赋值
## 元素的访问
```c++
std::cout << std::get<0>(t1) << std::endl;
```
访问元组元素的索引在运行期间不能改变，必须在编译阶段就已经确定
## 元组数量的获取
```c++
std::cout << std::tuple_size<decltype(t1)>::value << std::endl;
```
```decltype```可以返回表达式的类型