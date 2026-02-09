好了，我要上VS 2026了

那还说啥了，记录在代码里

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
	return 0;
}
```