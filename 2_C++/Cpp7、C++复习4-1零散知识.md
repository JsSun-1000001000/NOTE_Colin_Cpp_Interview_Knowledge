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

[c++ - “lambda 表达式”的优势是什么？\_Stack Overflow中文网](https://stackoverflow.org.cn/questions/18168022)
## 多继承

普通的继承中，子类的虚表是从父类拷贝过来的。子类新增加的特有的虚函数，会添加在这个虚表里。

多继承——继承多个父类
B公有继承A1和A2，意味着B同时有A1和A2父类的拷贝，同时具有A1，A2的特性
```cpp
class A1{};class A2{};
class B:public A1, public A2
{};
```
**多继承性质：**
- 虚表A1，A2都会有一份，B里面就应该有来自A1，A2的两个虚表，如果子类增加特有的虚函数，那么要在第一个虚表中添加。
![[多继承.png]]

**为了解决菱形继承——虚继承**

【挖坑。。。】

## 内部类

如果一个类B定义在另一个类A的内部，这个类B就叫做内部类。类A叫外部类（struct套struct占空间，类套类内部类不定义对象就不占用空间）

**性质：**
- 内部类是独立的，不属于外部类，更不能通过外部类的对象去调用内部类的所有成员
- 内部类就是外部类的友元类，内部类可以通过外部类的对象来访问外部类中的所有成员
- 外部类不是内部类的友元

**注意：**
- 内部类可以定义在外部类的public、protected、private都是可以的。如果内部类定义在public，则可以通过 ==外部类名::内部类名 来定义内部类的对象==。
- 内部类可以直接访问外部类中的static、枚举成员、不需要外部类的对象/类名
- ==sizeof(外部类) = 外部类，和内部类没有任何关系==

代码：
```cpp
#include<iostream>

using namespace std;

//什么是内部类?
//如果一个类B定义在另一个类A的内部，这个类B就叫做内部类。类A叫外部类

//性质
//内部类是独立的, 不属于外部类, 更不能通过外部类的对象去调用内部类
//内部类就是外部类的友元类, 内部类可以通过外部类的对象来访问外部类中的所有成员
//外部类不是内部类的友元

//注意
//1.内部类可以定义在外部类的public、protected、private都是可以的。
//如果内部类定义在public，则可通过 外部类名::内部类名 来定义内部类的对象。
//2.内部类可以直接访问外部类中的static、枚举成员，不需要外部类的对象/类名。
//3.内部类可以在外部类类外定义
//4.sizeof(外部类)=外部类，和内部类没有任何关系
//5.定义堆区内部类对象的方法 outer::inner * pi = new outer::inner;

class outer
{
public:
	class inner{
	public:
		void say( outer* p){
			cout << p->a <<endl;//可以访问因为内部类是外部类的友元类
			cout << s <<endl; // 可以直接访问外部类的static成员不用类名和对象
		}
	private:
		int i;
	};
	void speak( inner * ie){
		//cout << ie->i <<endl;//外部类不是内部类的友元类,不可访问
	}
private:
	int a;
	static int s;
	class private_inner;
};

//内部类可以在类外定义
class outer::private_inner{
		public:
		void say( outer* p){
			cout << p->a <<endl;//可以访问因为内部类是外部类的友元类
		}
	private:
		int i;
};


int main()
{
	outer o;
	outer::inner ir;
//	outer::private_inner pi; // 不可定义外部类对象
	//在堆中创建内部类
	outer::inner * pi = new outer::inner;
	delete pi;
	cout << sizeof( o ) <<endl; //结果4  内部类是类型不占空间
	getchar();
	return 0;
}
```

## auto

## auto 和decltype

auto：让编译器在编译时就推导出变量的类型，可以通过 = 右边的类型推导出变量的类型

`auto a = 10;` a是int

decltype：用于推导表达式类型，只用于编译器分析表达式的类型，表达式实际不会进行运算。

```cpp
const int & i = 1;
decltype(i) b = 2; // b是const int&
```
**对于decltype(exp)有：**
- exp是表达式，decltype(exp)和exp类型相同
- exp是函数调用，decltype(exp)和函数返回值类型相同
- 若exp是左值，decltype(exp)是exp类型的左值引用
```cpp
int a = 0, b = 0;
decltype(a+b) c = 0;
//c 是int 因为(a+b)返回一个右值
decltype(a += b) d = c;
//d 是 int& 因为a+=b返回一个左值
```
**auto与decltype组合使用：**
```cpp
template<typename T, typename U>
decltype(t+u)add(T t,U u){
	return t+u;
}//t和u尚未定义 无法通过编译

template<typename T, typename U>
auto add(T t, U u)->decltype(t+u){
	return t+u;
}
```
## for 基于范围的for循环
```cpp
int arr[10]={};
for(int num:arr){
	cout<<num<<endl;
}
```
![[C++11using.png]]