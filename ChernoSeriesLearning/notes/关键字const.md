## ```const```修饰函数
```const```可以用来修饰函数的参数：
```c++
returnType funcName (const paramList) { funcBody }
```
注意：
- 按值传递的参数无需使用```const```修饰，因为函数将自动产生临时变量用于存储按值传递的参数，因此无需使用```const```来防止该临时变量被修改
- 对于按引用传递的自动数据类型的参数，当不希望该函数修改该参数时，可以使用```const```予以修饰，当该参数被修改时，编译器将返回编译错误
```const```可以用来修饰函数的返回值：
```c++
const returnType funcName (paramList) { funcBody }
```
### ```const```修饰成员函数
当成员函数不会修改该类的成员时，应该用```const```予以修饰，其基本的用法为：
```c++
returnType funcName (paramList) const { funcBody }
```
注意：在使用被```const```修饰的成员函数时，该成员函数必须调用同样被```const```修饰的函数