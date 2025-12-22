
可变个数的参数列表怎么写？

```cpp
#pragma once

//printf实现
#include <stdarg.h>

int myprint(char* fmt, ... ){
	return 1;
}

/*---------------------------------------------*/

#include "myprint.h"

int main(){
	myprint("hello");
	return 0;
}

```
不知道参数就用 点点点

那怎么取参数呢
首先包含头文件
用来获取不确定个数的参数

VS_LIST
VA_START
VA_ARG
VA_END

```c

//VA_LIST 是在C语言中解决变参问题的一组宏，  va_list ap;
//所在头文件：#include <stdarg.h>，用于获取不确定个数的参数。
//VA_START宏，获取可变参数列表的第一个参数的地址
//（ap是类型为va_list的指针，v是可变参数最左边的参数）va_start(ap,v)
//VA_ARG宏，获取可变参数的当前参数，返回指定类型并将指针指向下一参数
//（t参数描述了当前参数的类型） va_arg(ap,t)
//比如下一个参数是 const char*  
//那么使用  const char* s = va_arg(ap, const char* );获取
//VA_END宏，清空va_list可变参数列表 va_end(ap) 

//putchar  打印字符  也可以使用  write  stdout 
//itoa  整数转换为字符串  itoa( 整数 , 缓冲区, 进制 )

#include <stdarg.h>
#include <stdlib.h>
#include <stdio.h>

int myprintf( char* fmt , ...)
{
	const char *s;
	int d;
	char buf[16];

	va_list ap;
	va_start(ap, fmt);
	while (*fmt) {
		if (*fmt != '%') {
			putchar(*fmt++);
			continue;
		}
		switch (*++fmt) {
			case 's':
				s = va_arg(ap, const char *);
				for ( ; *s; s++) {
					putchar(*s);
				}
				break;
			case 'd':
				d = va_arg(ap, int);
				itoa(d, buf, 10);
				for (s = buf; *s; s++) {
					putchar(*s);
				}
				break;
			case 'x':
				d = va_arg(ap, int);
				itoa(d, buf, 16);
				for (s = buf; *s; s++) {
					putchar(*s);
				}
				break;
				/* Add other specifiers here... */             
			default: 
				putchar(*fmt);
				break;
		}
		fmt++;
	}
	va_end(ap);
	return 1;   /* Dummy return value */
}
```