在C++中，当我们将一个对象赋值给另一个对象时，默认情况下，==编译器会为我们生成一个浅拷贝的赋值运算符==。这意味着赋值后的对象和原对象会共享同一个内存空间，这会导致和浅拷贝相同的潜在问题，特别是在一个对象被销毁时，另一个对象继续使用该内存区域会引发错误。

1. **自我赋值检查**：自我赋值是指对象在赋值时被赋值给自己，例如 s1 = s1。在这种情况下，如果我们没有进行检查，就会先删除对象的内存，然后再试图复制同一个对象的内容，这样会导致程序崩溃。因此，重载赋值运算符时，自我赋值检查是非常必要的。
2. **释放原有内存**：在分配新内存之前，我们必须先释放旧的内存，以防止内存泄漏。
3. **深拷贝**：通过分配新的内存，确保目标对象不会与源对象共享内存，避免浅拷贝带来的问题

那还说啥了，代码如下：

```cpp
/**
* @file main.cpp
* @data 2026-01-09-1.04
* @auther jssun
* @note make a string class by myself
*/
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstring>
#include <cassert>	//< 提供断言功能 用于在程序运行时检查某些条件是否为真，假会终止并输出错误信息

namespace W
{
	class string
	{
	public:
		//< 默认构造函数
		string(const char* str = "")
		{
			//< 初始化
			_size = strlen(str);
			_capacity = _size;
			_str = new char[_capacity + 1];
			// 正确的 strcpy_s 用法：strcpy_s(目标, 目标缓冲区大小, 源)
			strcpy(_str, str);
		}
		//< 深拷贝构造函数
		//深拷贝确认了每个对象都有独立的内存空间
		string(const string& s)
		{
			_size = s._size;
			_capacity = s._capacity;
			_str = new char[_capacity + 1];//分配新的内存
			strcpy(_str, s._str);//复制字符串内容
		}
		//赋值运算符重载
		string& operator = (const string& s)
		{
			/**
			* @note 
			* 1. 自我赋值检查，当对象自己给自己赋值的时候，
			* 如果没有进行检查，会**先删除对象的内存，然后再试图复制同一个对象的内容**
			* 这样会导致程序崩溃
			* 2.释放原有内存，分配新内存前，先释放原有内存，防止内存泄漏
			* 3.深拷贝防止浅拷贝带来的问题
			*/
			if (this != &s)//避免自我赋值
			{
				delete[] _str;	//释放原来内存
				_size = s._size;
				_capacity = s._capacity;
				_str = new char[_capacity + 1];//重新分配新内存
				strcpy(_str, s._str);//复制内容
			}
			return *this;
		}
		//< 析构函数
		~string()
		{
			if (_str)
			{
				delete[] _str;
				_str = nullptr;
			}
		}
		const char* c_str() const { return _str; }
	private:
		/**
		* @note 需要存储字符串的数组 字符指针
		* 分配的内存容量 8字节无符号整数
		* 当前字符串的有效长度 同上
		*/
		char* _str;
		size_t _capacity;
		size_t _size;
	};
}
/**
* @param 浅拷贝和深拷贝
* 当通过另一个string对象来构造新的对象，默认会发生浅拷贝，对象共享
* 同一块内存。当对象被销毁时，会导致多个对象试图释放同一块内存，进而
* 导致程序崩溃
*/
void TestString()
{
	W::string s1("asdfasdf");
	W::string s2(s1);//浅拷贝 s1 s2共享同一块内存 深拷贝都有独立内存
	//析构函数会尝试两次释放同一块内存，导致程序崩溃
	//析构的顺序：后构造的先析构，调试的话s2先析构然后是s1
}

int main() 
{
	W::string s("fuck you c++");
	TestString();
	std::cout << s.c_str() << std::endl;
	/////////////////////////////////////
	W::string s1("you c++");
	W::string s2("mother xxx");
	s2 = s1;//调用赋值运算符重载
	std::cout << s2.c_str() << std::endl;
	return 0;
}
```