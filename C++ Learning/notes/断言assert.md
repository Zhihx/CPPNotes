## 静态断言

静态断言```statci_assert```的使用目的是希望在<mark style="background: #FFB86CA6;">编译阶段</mark>就能发现更多错误，这是我们使用编译器在编译阶段立下的“契约”，其使用要点有：
- 静态断言的第一个参数必须是<mark style="background: #FFB86CA6;">常量表达式</mark>
- 静态断言的第二个参数是当断言失效后提示的字符串
## 运行期断言

