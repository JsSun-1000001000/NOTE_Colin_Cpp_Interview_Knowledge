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

## 类模板

类模板参数C++11之前，类模板就可以加默认参数，不过默认值要从右向左定义
```cpp
template <typename T, typename U = int>
class A{
	T value;
}
template<typename T = int,typename U>
class A{
	T value;
}
```

C++11之前没有函数模板默认值，C++11之后可以了，并且默认值定义没有从右到左的限制
```cpp
template <typename R=int, typename U>
R func2(U val){
	return val;
}
```

## std::function 函数封装器

满足以下条件之一就可以称为可调用对象：
- 是一个函数指针
- 是一个具有operator()成员函数的类对象（传说中的仿函数）
- 是一个lambda表达式
- 是一个可被转换为函数指针的类对象
- 是一个类（函数）成员指针

bind表达式或其他函数对象
std::function就是上面这种可调用对象的封装器，可以把std::function看作一个函数对象用于表示函数这个抽象概念，实现函数的动态绑定和传递

```cpp
#include <functional>

struct PrintNum{
	void operator()(int i)const{std::cout<<i<<endl;}
};

PrintNum print_num;
std::funciton<void(int)> f_display = print_num;//调用对象封装到f_display中
f_diaplay(-9);

注意：若是std::funciton不含目标，则称它为空，调用空的std::function的目标会抛出std::bad_function_call异常
```
## std::bind 

问的频率不是很高，**右值引用要好好看**

使用std::bind可以将可==调用对象和参数==一起绑定，绑定后的结果使用std::function进行保存，并延迟掉用到任何我们需要的时候。通过std::bind，可以在调用函数的时候提前绑定部分参数，从而简化函数调用的过程，有助于提高代码的可读性和灵活性。

通常有两大作用：
- 将可调用对象与参数一起绑定为另一个stdfunction供调用
- 提前绑定部分参数，简化函数调用

## C++关于线程并发

- thread相关
- mutex相关
- lock相关
- atomic相关
- call_once相关
- condition_variable相关

## C++11委托构造

```cpp
struct A{
	A(){}
	A(int a){a_ = a;}
	A(int a, int b):A(a) {b_ = b;}
	A(int a, int b, int c):A(a,b){c_ = c;}
	int a_;
	int b_;
	int c_;
};
```

## C++继承构造函数

```cpp
struct base{
	base(){}
	base(int a){a_ = a;}
	base(int a, int b):base(a){b_ = b;}
	int a_;
	int b_;
	int c_;
};
struct derived:base{
	derived(){}
	derived(int a):base(a){}//好麻烦
	derived(int a, int b):base(a, b){}//好麻烦
}

struct derived:base{
	using base::base;
	//使用 这个继承构造函数
}
```
## C++使用nullptr表示空指针，而不是NULL因为后者是0
```cpp
void func(void * ptr){
	cout<<"func ptr"<<endl;
}
void func(int i){
	cout<<"func i"<<endl;
}

int main(){
	func(NULL);// func i
	func(nullptr);// func ptr;
}
```

## C++11 final

final修饰的类不可以被继承
```cpp
struct base final{
	virtual void func(){
		cout<<"base"<<endl;
	}
};
struct derived:public base{
	//编译失败 final修饰的类不可以被继承
	void func()override{
		cout<<"derived"<<endl;
	}
	void fu()override{}//error 基类没有fu 不可以被重写
};
```
## C++default 和 delete
```cpp
struct a{
	a() = default;//自动生成默认构造
	int aa;
	a(int i){aa = i;}
};
int main(){
	a aa;
	return 0
}
/////////////////////////////////////////
struct a{
	a() = default;
	a(const a&) = delete;//禁用拷贝构造
	a& operator = (const a&) = delete;//禁用
	int a;
	a(int i){a=i;}
};
int main(){
	a a1;
	a a2 = a1; //错误 拷贝构造函数被禁用
	a a3;
	a3 = a1;   //错误 拷贝赋值操作符被禁用
}
```
## ex'plicit 专用于修饰构造函数，表示只能显示构造，不可以隐式转换
```cpp
struct a{
	a(int value){//没有explicit
		cout<<"value"<<endl;
	}
};
int main(){
	a a1 = 1;//可以隐式转换
	return 0;
}
//////////////////////////////////////////////////
struct a{
	explicit a(int value){
		cout<<"value"<<endl;
	}
};
int main(){
	a a1 = 1;// error 不可以隐式转换
	a a2(2);//ok
	return 0;
}
```
## constexpr 

constexpr是C++新引入的关键字，用于编译时的常量和常量函数，==**constexpr修饰的是真正的常量，会在编译期间就被计算出来，整个运行过程中不可以被改变**==，可以用于修饰函数，返回值会尽可能在编译期间被计算出来当作一个常量，如果编译期间此函数不能被计算出来，那他就会当作一个普通函数被处理

```cpp
constexpr int func(int i){
	return i+1;
}//这个结果可以给const赋值

int main(){
	int i = 2;
	func(i);//普通函数
	func(2);//编译期间就会被计算出来
}
```

**==const和constexpr的区别：==**
- 

