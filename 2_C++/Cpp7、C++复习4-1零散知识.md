## lambda表达式

背景：
```cpp
#include <algorithm>
int fun(int a,int b){
	return a>b;
}
vector<int> vec{2,1,4,6,4,9};
sort(vec.begin(),vec.end());//小到大
sort(vec.begin(),bec.end(),&fun);
```
函数里面定义一个函数，匿名函数

函数作为一个参数时，需要在函数外写函数的实现，以前无法在函数内实现另一个函数

lambda表达式，定义匿名函数，又称lambda函数，可以在函数内部实现函数定义。

**auto 函数名= lambda表达式；可以通过函数名，调用函数**

`sort(vec.begin(),vec.end(),[](int a,int b){return a > b;};//作为匿名函数使用`

### lambda表达式具体语法：以\[]作为开始标志

`[]( )mutable->type{ }` 从左到右
- 捕获列表
- 参数列表
- 是否是常函数 mutable可变的
- 返回值类型
- 函数体
