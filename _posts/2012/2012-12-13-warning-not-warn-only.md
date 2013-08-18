---
layout: post
title: 编译器的warning往往不只是警告
categories: 
- Programming
tags: 
- Program, C Language
---


编译警告warning是所有的程序员都不可能不会遇到的一件事情，而且往往在一个很大的project里面这更是司空见惯的事情，如果那一天你敲下Make的瞬间少了那些一串串的“warning：”，你可能反而会怀疑是不是哪儿出来问题/_\  虽然说每一个人的编程习惯，个人素养不一样，对待warning的态度也不尽一致，但是总得来说80%的程序员都是抱着“warning is okay”的态度；追根溯源，为什么编译器会有Error和warning之分呢？通俗的来说，编译器确定不允许的就认为是Error，然后一些违背原则但是编译器又不确定的就定义为warning，既然是不确定，那就有一个限度的概念，所以编译器都允许定制warning的级别，但是建议在Makfile中打开“Wall"的选项，这样让更多的问题暴露无遗。所以说warning是编译器为程序员提供的友善建议和意见，即便编译器这么热心和蔼，但是仍然有很多程序员对Warning不以为然。下面就简单总结几个GNU GCC为我们编译时候发生warning的情况下，如果你不以为然，那么你将在稍后运行的时候变得很迷茫，苦恼和尴尬……

__Case 1__

```c
/* this is main.c */
int main(int argc, char **argv)
{
    foo();
 
    return 0;
}
 
/*This is foo.c*/
int foo(char *s, int len)
{
    *(s + len) = 0;
 
    return 0;
}
```    
```
Kevins-MacBook-Pro:Test kinreven$ gcc -c -Wall main.c -o main.o && gcc -c -Wall foo.c -o foo.o
main.c: In function ‘main’:
main.c:3: warning: implicit declaration of function ‘foo’
Kevins-MacBook-Pro:Test kinreven$ gcc main.o foo.o -o main.exe
```
上面的implicit declaration 估计是最常见过的warning之一，这种问题多出在模块于模块之间缺少header file的沟通，或者直接调用其他模块内部的function造成。有时候是因为漏include了该模块的header file，有时候是由于架构设计的问题导致需要的interface没有暴露出来，但是更更多情况是由于debug或者一次hack需要直接call到其他模块内部。这样即便是你的调用形式和实现的函数原型冲突都不会引发编译错误。因为在compile时候，都是依据header file里面申明的函数原型和调用进行check，如果没有函数的申明，那么compiler仅仅是抛出”implicit declaration“的warning，而在Link的时候，只要其他lib里面能够找到这样的函数名，那么根据符号匹配就能Link成功。但是当你在运行时候，如上面的case就要引发诸如”段错误“等问题导致crash。所以正确的做法应该是include其他模块的header file，这样如果调用的时候参数类型和个数不匹配便会发生Compile Error。

__Case 2__

```c
/* this is main.c */
char a,b;
void f(int *a, int *b)
{
    int t;
    t = *a;
    *a = *b;
    *b = t;
}
void g(void)
{
    f(&a, &b);
}
```    
```
Kevins-MacBook-Pro:Test kinreven$ gcc -Wall main.c -o main.exe
main.c: In function ‘g’:
main.c:12: warning: passing argument 1 of ‘f’ from incompatible pointer type
main.c:12: warning: passing argument 2 of ‘f’ from incompat
```	
上面的incompatible type又是一常见warning，这样的问题在大多数情况下面应该是okay的，因为C会进行隐式类型转换，但是像上面的case估计就踩到雷区了，可能他的输出就未必是你想要的了:> 因为a，b是char类型占1byte，而f()的两个pointer都是int *，所以在里面进行调换的时候就会发生覆盖的情况，要想知道结果就自己试试吧

__Case 3__

```c
/* this is main.c */
void f(void)
{
    char c;
    for (c=0; c<=255; ++c) {
    /* ... */
    }
}
```    
```
Kevins-MacBook-Pro:Test kinreven$ gcc -Wall main.c -o main.exe
main.c: In function ‘f’:
main.c:5: warning: comparison is always true due to limited range of data type
```	
上面的warning写的很清楚，但是你如果不看估计也未必能发现你是多么的傻，估计在C的第一章节就会讲到常用的数据类型，然后老师还会强调每一种数据类型的长度，char在大多数的系统里面都应该是1byte，所以他的取值区间是-128 ~ 127,所以这里的++c 无论如何都是<255 ,所以这里永远都是ture，这样就产生了你不预期的死循环。

__Case 4__    

```c
/* this is main.c */
int *a, *b;
void f(int delta, int base)
{
    unsigned int u = a - b;
    if (u - delta < base) {
        ; /*...*/
    }
}
```    

上面function在比较旧的GCC版本上面是会产生一条”comparison between signed and unsigned integer expressions“的警告，但是我在罪行GCC 4.x上面在加"-Wall -Wconversion -ansi"选项后均为产生warning信息。但是上面的隐式转换往往会给你带来非预期的结果，比如上面的delta盒unsigned ，然而delta是int，所以在表达式u - delta中，delta会隐式的转换成unsigned，如果delta是一个负数就会转换为一个很大的整数，这样的结果可能并不是你期望的/_\

> So，我们应当认真对待warning，如果你不以为然，有时候他会让你后悔；养成好习惯，清除warning，让built windows更干净，让compiler更happy。

