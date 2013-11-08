---
layout: post
title: "解题报告-字符串变换的面试题"
---

#### 题目描述

在实际的开发工作中，对字符串的处理是最常见的编程任务。本题目即是要求程序对用户输入的串进行处理。具体规则如下： 

   * 把每个单词的首字母变为大写。 
   * 把数字与字母之间用下划线字符（`_`）分开，使得更清晰 
   * 把单词中间有多个空格的调整为1个空格。 

例如:  
  用户输入:   
  `you and     me what  cpp2005program`   
  则程序输出:   
  `You And Me What Cpp_2005_program`   
  用户输入:  
  `this is     a      99cat`   
  则程序输出:   
  `This Is A 99_cat`   
        
假设:
  * 用户输入的串中只有小写字母，空格和数字，不含其它的字母或符号。每个单词间由1个或多个空格分隔。  
  * 用户输入的串长度不超过200个字符。   

#### 解题分析


#### 参考代码

```c
#include <stdio.h> 

enum { 
    WORD_FIRST = 0, 
    WORD_SPACE, 
    WORD_NUMBER, 
    WORD_OTHERS, 
};

#define ISDIGIT(x)   ((x) >= '0' ? ((x) <= '9' ? 1 : 0) : 0) 
#define ISLCHAR(x)   ((x) >= 'a' ? ((x) <= 'z' ? 1 : 0) : 0) 
#define ISSPACE(x)   ((x) == ' ')
#define UPCASE(x)    ((x) - 32)

int posjudge(char p, char n) 
{
    int pos;
    
    if (ISSPACE(p) && ISLCHAR(n)) { 
        pos = WORD_FIRST; 
    } 
    else if (ISSPACE(p) && ISSPACE(n)) { 
        pos = WORD_SPACE; 
    } 
    else if (ISLCHAR(p) && ISDIGIT(n)) { 
        pos = WORD_NUMBER; 
    }
    else if (ISDIGIT(p) && ISLCHAR(n)) {
        pos = WORD_NUMBER;
    }
    else { 
        pos = WORD_OTHERS; 
    }

    return pos; 
}

void transform(char *in, char *out) 
{ 
    int pos = ISLCHAR(*in) ? WORD_FIRST : WORD_OTHERS; 
    char *pre = in;
    
    while (*in != '\0') { 
        switch (pos) { 
        case WORD_FIRST: 
            *out++ = UPCASE(*in++); 
            pos = posjudge(*pre++, *in); 
            break; 
        case WORD_SPACE: 
            in++; 
            pos = posjudge(*pre++, *in); 
            break; 
        case WORD_NUMBER: 
            *out++ = '_'; 
            *out++ = *in++; 
            pos = posjudge(*pre++, *in); 
            break; 
        case WORD_OTHERS: 
            *out++ = *in++; 
            pos = posjudge(*pre++, *in); 
            break;
        default:
            break;
        } 
    } 
} 

int main(int argc, char *argv[]) 
{ 
    char in[200] = "9this is     a  999 bcd0    99cat   "; 
    char out[200]; 

    printf("in:%s\n", in); 
    transform(in, out); 
    printf("out:%s\n", out); 
    
    return 0; 
}
```

