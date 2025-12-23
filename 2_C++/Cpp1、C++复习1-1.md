## C语言复习阶段遗留问题

关于结构体空间分配的调试代码如下：
（怎么设计例子，看结构体空间是如何分配的？）
```cpp
#include <stdio.h>
#include <stdlib.h>

struct node{
	short s;
	char c[7];
	double d;	
}

struct subdata{
	int a;
	char * b;
}

struct data{
	int a;
	char b;
	int *c;
	int d;
	struct subdata * e;
	char cs;
	struct subdata f;
}

int i = 0;
struct node * p= &n;

n.c[0] = 1;
n.c[1] = 1;
n.c[2] = 1;
n.c[3] = 1;
n.c[4] = 1;
n.c[5] = 1;
n.c[6] = 1;
n.s = 1;
n.d = 1;

printf("%d\n",sizeof(n));

a = sizeof(struct data);

int main(){
	return 0;
}
```
下断点后，调试里调出内存窗口
## C环境下实现一个函数，用来交换，可以是两个int交换，也可以是两个char交换，可以是其他相同类型的两个变量交换

1. 传参，传的是值还是地址？——地址；C语言里写一个通用类型（泛型）——`void *`
2. 
