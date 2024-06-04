## Static for variables

**Static variables in a Function**: When a variable is declared as static, space for **it gets allocated for the lifetime of the program**. Even if the function is called multiple times, space for the static variable is allocated only once and the value of the variable in the previous call gets carried through the next function call:
- static修饰的变量将在**整个程序的生命周期**持续，即程序结束后才被释放。则当该变量存在于某函数中时，前一次调用该函数，函数内该变量值将延续到下一个函数调用。
- static修饰的变量将只被初始化一次

**Static variables in a class**: As the variables declared as static are initialized only once as they are allocated space in separate static storage so, the static variables **in a class are shared by the objects.** There can not be multiple copies of the same static variables for different objects. Also because of this reason static variables can not be initialized using constructors：
- static修饰的变量在类中时，该变量将被该类的所有对象共享，因此，该变量不允许在构造函数中被初始化
- static修饰的变量在类中时，允许使用```typeName className::varName```的方式初始化该变量（注意初始化需要在main外）

## Static for functions in a class

**Static functions in a class**: Just like the static data members or static variables inside the class, static member functions also do not depend on the object of the class. We are allowed to invoke a static member function using the object and the ‘.’ operator but it is recommended to invoke the static members using the class name and the scope resolution operator. **Static member functions are allowed to access only the static data members or other static member functions**, they can not access the non-static data members or member functions of the class.
- 静态成员函数只允许访问静态成员数据或其他静态成员函数
- 静态成员函数推荐使用```className::staticMemberFunc```

https://www.geeksforgeeks.org/static-keyword-cpp/


