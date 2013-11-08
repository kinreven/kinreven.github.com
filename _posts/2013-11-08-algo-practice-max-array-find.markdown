---
layout: post
title: "解题报告-排序数组中重复最多的数字长度"
---

#### 题目描述 

在 1,2,2,3,3,3,4,5,5,6中1,2.2,3.3.3,4,5.5,6都是平台。试编写一个程序，接受一个数组，把这个数组中最长的平台找出来。在上面的例子中就是3.3.3就是该数组中最长的平台。 

#### 解题分析 

#### 参考代码 

```cpp 
#include <stdio.h> 

int find(int *array, int size) 
{ 
    int max = 0; 
    int i, l; 
    int *p, *q; 

    p = array; 
    q = array + 1; 
    l = 1; 
    for (i = 1; i < size; i++) { 
        if (*p++ == *q++) { 
            l++; 
        } 
        else { 
            max = l > max ? l : max; 
            l = 1; 
        } 
    } 

    return max; 
} 

int main(int argc, char *argv[]) 
{ 
    int a[] = {1, 2, 2, 2, 2, 2, 3, 3, 3, 4, 5, 5, 6}; 

    printf("the max array size is %d\n", find(a, sizeof(a) / sizeof(a[0]))); 
    
    return 0; 
} 
``` 
