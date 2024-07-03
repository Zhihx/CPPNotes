## Sequence Containers

- vector（fast for push_back and element access, slow for push-front）
- deque（fast for push_back and push_front, slow for element access）
- list
- array
- forward_list

注意：
vector使用索引访问其中的元素时，不会进行bound check。在mac中对于超出边界的访问行为不会阻拦，在windows中则会“安静”地访问失败，例如：
```c++
#include <iostream>
#include <vector>
int main()
{
    std::vector<int> vec(3);
    for (size_t i = 0; i < 3; i++)
    {
        vec[i] = 3;
    }
    
    for (size_t i = 0; i < 4; i++)
    {
        std::cout << vec[i] << std::endl; // output 3 3 3 0
    }
}
```

## Associate Containers

- map（sorted keys, faster to iterate through a range of elements）
- set（sorted keys, faster to iterate through a range of elements）
- unordered_map（hash function, faster to access individual elements by key）
- unordered_set（hash function, faster to access individual elements by key）
## Container Adaptors


## Iterators 

具体参见[[迭代器Iterators]]

