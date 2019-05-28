# C语言指针
## 关于地址编号
地址编号在32位系统下，是一个4个字节的无符号整数，在64位系统下是一个8个字节的无符号整数；在同一个系统下，不管指向什么样类型的变量，地址的大小总是一样的。
## 指针
指针保存的是地址编号，指向的是变量的首地址，所以不管什么类型的指针变量其大小在同一操作系统下都是一样的。
```c
int *p;//定义了一个int类型的指针
char *p;//定义了一个char类型的指针
...
void *p;//定义了一个可以指向任意类型地址的指针
```
## 野指针和空指针
### 野指针
```c
int *p;//没有初始化过值得指针，这种指针叫野指针
*p = 100;//在p指针没有初始化之前，该操作会报`Segmentation fault: 11`，
```
如上代码，我们定义了一个int类型的指针p，但是我们没有对它进行初始化，在此时p就是**野指针**，接着我们第二行又对*p（指针p所指的内存）进行赋值，这样的操作是不被允许的，编译器会报Segmentation，在开发中我们应该避免出现野指针这种情况，更不应该使用野指针。
### 空指针
```c
int *p；
p = NULL;
```
如上代码所示，如果一个指针变量没有明确的指向一块内存，那么就把这个变量指向NULL，这个指针就是空指针。
```c
#define NULL 0
```
NULL在c语言里面是一个宏常量，值是0，**程序中不要出现野指针，但可以出现空指针**。
### warning 指针之间类型一定要匹配（切记）
```c
int *p;
p = NULL;
int a;
char b;
b = 30;
a = b;
p = &b;//warning,指针之间类型一定要匹配
```
### 指向常量的指针和指针常量
> **c语言中的const是有问题的，因为可以通过指针变量间接的修改const常量的值，所以在c语言中用#define常量的时候更多。**    
`const int *p;`，该p是一个变量,但指向一个常量 即指向常量的指针；`int *const p;`，该p是一个常量，但指向一个变量或者常量 即指针常量。
```c
void point_const(){
	const int *p;//p是一个变量，但指向一个常量
	int a，b;
	p = &a;
	// *p = 20;//error,指向常量的指针不能通过*p修改a的值,但可以通过*p访问a的值，也能修改p所指向的地址
	p = &b;//指向常量的指针是可以改变所指变量的
	int *const p1 = &a;
	// p1 = &a;//error,指向常量的指针，不能修改p1所指向的地址，但可以使用*p修改a的值
	printf("%d\n", *p);
}
```
### 指针与数组
**当指针变量指向一个数组的时候，C语言语法规定指针变量名可以当数组名使用**，但是两者不能划等号，比如：
```c
int main()
{
	int a[10] = {1,2,3,4,5,6,7,8,9,10};
	int *p;
	p = a;
	p[3] = 100;//指针名当数组名使用在c语言中是合法的
	printf("%lu, %lu\n", sizeof(a), sizeof(p));//这里结果是40，8，由此可证明它们不是完全一样
	int i;
	for(i = 0; i < 10; i++)
	{
		printf("a[%d] = %d\n",i,p[i]);
	}
	return 0;
}
```
### 指针运算
指针可以用被用来做运算，但是只能做加减运算，其运算规律因类型而定，比如：
```c
void ys_point()
{
	int a[10] = { 0 };
	int *p = a;
	p += 5;//此时指针指向的是第六个元素
	*p = 1;//将第六个元素的值赋值为1
	p -= 2;//指针向左移动两个位置，指向的是第四个元素
	*p  = 3;//将第四个元素的值赋值为3
	for (int i = 0; i < 10; ++i)
	{
		printf("a[%d] = %d\n", i, a[i]);
	}
}
```
*特别强调，C语言中任何数据都能用char来表示*
*一个小例子：*将一个int类型树转换为ip地址
```c
void test_point_ip()
{
     int ip = 98767282;
     unsigned char *p = (unsigned char *)&ip;
     printf("%d\n",ip);
     int i;
     for( i = 0; i < 4; i++ ) {
         printf("%u\n",p[i]);
     }
     printf("%u.%u.%u.%u\n", p[3],p[2],p[1],p[0]);
 }
```
### 数组指针
数组指针跟普通的数组基本类似，如下：
```c
//数组指针
void test_point_array()
{
    char *a[10];//定义了一个指针数组，每一个成员是char*类型，一共10个成员
    int *b[10];
    printf("%lu,%lu\n", sizeof(a), sizeof(b));
    int i = 0;
    //a = &i;//error，不能这样赋值，数组名不能作为左值
    b[0] = &i;
    printf("%lu, %lu\n", sizeof(b[0]), sizeof(*b[0]));
}
//数组指针2
void test_point_2array()
{
    int *pa[10] = { NULL };
    int a, b, c;
    pa[0] = &a;
    pa[1] = &b;
    pa[2] = &c;
    *pa[0] = 10;
    printf("%d,%d,%d\n", a, b, c);
}
```
### 二级指针/指向指针的指针
```c
//二级指镇/指向指针的指针
void test_point_point()
{
    int a = 0;
    int *p = &a;
    int **pp;//定义一个指向指针的指针，只能存放指针的地址
    pp = &p;
    **pp = 10;//这里表示对a赋值10
    printf("%d\n", a);
}
```
指向指针的指针只能存放指针的地址。
能用一级指针解决的问题不要二级，能用二级解决的不用三级，指针级数过多会导致程序很复杂。
### 函数参数
c语言想通过函数内部来修改实参的值，只能给函数传递实参的地址来间接的修改实参的值。
```c
int a;
scanf("%d", &a);//scanf要改变a的值，只能用传递a的地址的方式修改a的值
```
*当一个数组名作为函数的形参的时候，其实是一个指针变量名*
### 参数数组
```c
void print_array(int size,int *a)
{
    int i;
    for (i = 0 ; i < size; i++) {
        printf("%d\n",a[i]);
    }
}
```
如果一个数组作为函数的参数，那么数组的成员数量在函数内部是不可见的，在传递一个数组的时候，同时提供另外一个参数，标明这个数组有几个成员变量。如果函数的参数是一个字符串，那么并不需要再传递一个参数说明这个字符串有多长。
### 代码
```c
//
//  main.c
//  C语言指针复习
//
//  Created by LE on 2018/4/25.
//  Copyright © 2018年 LE. All rights reserved.
//

#include <stdio.h>

//指针。。。
void point_hello()
{
    int *p;
    int a;
    a = 1;
    p = &a;
    printf("%p\n", p);
    printf("%d\n", *p);
    void *p1;//可以指向任意类型地址的指针
}
//空指针和野指针
void null_point()
{
    // int *p;//没有初始化过值得指针，这种指针叫野指针
    // *p = 100;//在p指针没有初始化之前，该操作会报`Segmentation fault: 11`，
    int *p;
    //p = NULL;//如果一个指针变量没有明确的指向一块内存，那么就把这个变量指向NULL，这个指针就是空指针，
}

//指针变量一定要类型匹配
void point_type(){
    int *p;
    p = NULL;
    int a;
    char b;
    b = 30;
    a = b;
    // p = &b;//warning,指针之间类型一定要匹配
}

//指针常量和指向常量的指针
void point_const(){
    const int *p;//p是一个变量，但指向一个常量
    int a, b;
    p = &a;
    // *p = 20;//error,指向常量的指针不能通过*p修改a的值,但可以通过*p访问a的值，也能修改p所指向的地址
    p = &b;//指向常量的指针是可以改变所指变量的
    int *const p1 = &a;
    // p1 = &a;//error,指向常量的指针，不能修改p1所指向的地址，但可以使用*p修改a的值
    printf("%d\n", *p);
}

//指针可以用被用来做运算，但是只能做加减运算，其运算规律因类型而定，比如：
void ys_point()
{
    int a[10] = { 0 };
    int *p = a;
    p += 5;//此时指针指向的是第六个元素
    *p = 1;//将第六个元素的值赋值为1
    p -= 2;//指针向左移动两个位置，指向的是第四个元素
    *p  = 3;//将第四个元素的值赋值为3
    for (int i = 0; i < 10; ++i)
    {
        printf("a[%d] = %d\n", i, a[i]);
    }
    
    int b = 0x12345;
    
    printf("%x\n", b);
}

void test_point_ip()
{
    int ip = 98767282;
    unsigned char *p = (unsigned char *)&ip;
    printf("%d\n",ip);
    int i;
    for( i = 0; i < 4; i++ ) {
        printf("%u\n",p[i]);
    }
    printf("%u.%u.%u.%u\n", p[3],p[2],p[1],p[0]);
}

//数组指针
void test_point_array()
{
    char *a[10];//定义了一个指针数组，每一个成员是char*类型，一共10个成员
    int *b[10];
    printf("%lu,%lu\n", sizeof(a), sizeof(b));
    int i = 0;
    //a = &i;//error，不能这样赋值，数组名不能作为左值
    b[0] = &i;
    printf("%lu, %lu\n", sizeof(b[0]), sizeof(*b[0]));
}
//数组指针2
void test_point_2array()
{
    int *pa[10] = { NULL };
    int a, b, c;
    pa[0] = &a;
    pa[1] = &b;
    pa[2] = &c;
    *pa[0] = 10;
    printf("%d,%d,%d\n", a, b, c);
}
//二级指镇/指向指针的指针
void test_point_point()
{
    int a = 0;
    int *p = &a;
    int **pp;
    pp = &p;
    **pp = 10;//这里表示对a赋值10
    printf("%d\n", a);
}
//耳机指针与指针数组
void test_2point_array()
{
//    int *a[10];
//    int **P = a;
}
//指针作为函数的参数
void swap(int *a,int *b)
{
    int temp = *a;
    *a = *b;
    *b = temp;
}
//不可变的指针参数
int mystrlen(const char *a)
{
    int len = 0;
    while (a[len]) {
        len++;
    }
    return len;
}
//打印数组,在函数内部是无法获得形参数组的大小的,字符串除外，因为字符串是以\0结尾的
void print_array(int size,int *a)
{
    int i;
    for (i = 0 ; i < size; i++) {
        printf("%d\n",a[i]);
    }
}
//指针数组案例:打印一数组指针
void print_point_array(int n,const char **p)
{
    int i;
    for (i = 0; i < n; i++) {
        printf("%s\n",p[i]);
    }
}

//关于main函数参数解释
/*
 第一个参数 argc 代表指针数组argv的长度
 第二个参数 argv 代表参数数组，是一个指针数组
 */
int main(int argc, const char * argv[]) {
    // insert code here...
    char *str[3];
    char s1[] = "hello";
    char s2[] = "world";
    char s3[] = "endl;";
    str[0] = s1;
    str[1] = s2;
    str[2] = s3;
    print_point_array(sizeof(str)/sizeof(str[0]), str);
    return 0;
}
```

