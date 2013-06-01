---
layout: post
title: "那些年我们一起追过的Shell Script"
description: ""
category: 程序人生
tags: [Program, Shell Script]
---
{% include JB/setup %}

[![shell_introduce_ppt][img0]][link0]

原本这是自己在几个月前为公司的一个分享活动写的一个投影片，今天趁大脑负荷比较小，把这个话题拿到blog上面来分享一下。从知道shell算起至今也就几个年头而已，如今勉强算是入门了。对某一个新事物的掌握总是一个循序渐近的过程，只是根据不一样事物的特点，其学习的曲线也不尽一致。shell script算是一门古老的脚本语言，最初始于UNIX/LINUX的系统管理中，用户透过shell可以将自己的命令传递给系统的内核，当系统完成你的命令后返回执行的结果给用户，所以shell最初意义上被定义为用户的操作系统的一个交互界面。当用户总是执行一连串重复的命令的时候，便思考能不能将这些重复的命令按照一定的格式组织在一个文本里面，然后每一次通过去解释执行这样的文本来实现一连串的命令，于是shell script便应运而生。这便是计算机早期的批次处理的工作方式，如同Windows下面的batch（当然名字叫batch的程序并非一行行的去执行，其实他和shell script一样具有脚本语言的语法，只是要简单和生硬得多而已。

上面说到shell script的学习曲线，我个人觉得前期的一些背景知识的学习是必不可少的，所以在学习的初期可能给你带来一定的困难，当你一旦掌握了其原理和拥有了一点实际的经验，那么你就会觉得简单多了，但是你要想成为像那些Linux脚本天才一样，能够将它运用到极限，那不言而喻。对于一个嵌入式、Linux/Unix程序员或者计算机系统管理员，shell script总是一门必须课程，所以我们应该趁早学习，免得许多年后你发现shell script的强大时候才感觉“相见恨晚”。我们应该如何学起呢？ 不像其他的高级语言，如C Languge，他们总是具有很好的跨平台性，无论是你使用Windows，还是使用Linux，或者是Mac OS，你都可以很容易的获取开发环境进行开发。shell script和Unix/Linux与生俱来，所以你要学习前总不可能避免的回去学习Linux。或许你是一个被MS windows毒害至深的TX，根本没听过Linux，抑或仅仅听过Linux，又或者是用过但是觉得超级难用… 诸如此类，都要克服人的惰性去学习，人总是习惯自己习惯的事物，改变自己习惯的习惯总是很难，而且还对违背自己习惯的事物非常的抵制。这里并不是比较Windows vs Linux，这是一个没有结论的争论。所以劝导大家在习惯自己习惯之前先养成一个好的习惯。

前面有关无关的话讲了许多，接下来简单介绍一下shell script，shell script你可以认为他就是一门高级的脚本语言，shell就是他的解释执行器，然后许许多多的系统自带的命令你可以想象成为是标准函数库。对于解释器，在大多数的posix系统里面都将Bash作为默认的shell，所以这里的shell script的语法也默指Bash的语法规则。Bash，你在大多数的Linux或者Unix 发行版中都应该可以找到，而且你一样可以从Windows下面的Unix Like环境Cygwin下面打开。然后shell script的语法部分内容都在我的投影片里面有讲述(请点击上面的图片获取共享的pdf)。下面分享一个简单的shell script实例：

大部分的程序员估计都会使用SVN作为版本控制工具（或许一些老顽固还在使用VSS，或许我自己out，大部分都在使用Git了），那在TortoiseSVN 1.7之前你总有为工程目录下面到处都是.svn的目录而苦恼过，或许你早已经google了一些小的tool可以解决这个问题，但是我这里想介绍的是通过shell script也很容易实现诸如此类的需求：  
``` shell
	#!/bin/bash
	CleanSVN () {
    FILES=`ls -a`;
    for FILE in $FILES
    do
        if [ -d $FILE ]; then
            if [ "$FILE" = ".svn" ]; then
                dir=`pwd`;
                echo "$dir/$FILE";
                rm -rf $FILE;
            elif [ "$FILE" != "." ] && [ "$FILE" != ".." ]; then
                cd $FILE;
                CleanSVN;
                cd ..;
            fi
        fi
    done
	}
	CleanSVN;
```    
[img0]:https://www.evernote.com/shard/s65/sh/6423ba85-6afb-4508-836e-d66e506e5eb1/a6a981f9aba8b2de6e47581b6c3015bd/deep/0/Screenshot%205/25/13%209:26%20PM.png

[link0]:https://docs.google.com/file/d/0B9ezmBSEh8NBTG1FZkdwdUhmcUE/edit?usp=sharing