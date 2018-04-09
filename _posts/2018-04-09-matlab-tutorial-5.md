---
layout: post
title:  "同学，你掉了一份MATLAB入门教程(5)"
categories: MATLAB
tags:  MATLAB 函数
author: 风之筝
---

* content
{:toc}

在我们已经可以编写简单的MATLAB脚本之后，我们需要学习一个更加强大的编程工具：函数。这篇文章将主要介绍MATLAB中函数的使用。




## 为什么需要函数

之所以说函数是一种强大的工具，是因为使用函数可以让我们的代码变得更加简洁，同时也给我们模块化带来了很大的方便。

以我们在生活中做家务为例，正常情况下，我们需要具体写出做家务的每一步，例如：

```
1. 擦桌子
2. 洗碗
3. 扫地
4. xxxx
5. 擦桌子
6. 洗碗
7. 扫地
8. ...
```

如果我们将其组织成一个函数，那我们每次只需要调用做家务这个函数，就可以实现这样一套流程。

```
定义：做家务 = 
	1. 擦桌子
	2. 洗碗
	3. 扫地

1. 做家务
2. xxxx
3. 做家务
4. ...
```

同样，如果我们需要在做家务的过程中添加一项“倒垃圾”，我们只需要在函数端进行修改，而不用对调用部分的代码做任何改动，这样模块化的方法将使得我们代码的可维护性大大增强。

## 函数定义

了解了函数的作用之后，我们就需要了解如何对函数进行定义。MATLAB中的函数定义包括function保留字、输出变量、函数名、输入参数、函数体几部分：

``` matlab
% function为保留字，out1~outN为函数输出，function_name为函数名，para1~paraN为输入参数
function [out1, out2, outN] = function_name(para1, para2, paraN)
	statements
end
```

定义函数时，一般通过建立单独的函数m文件进行编写。例如，我们可以定义一个“给定两个数，返回其和”的函数add。因此我们可以首先选择新建 -> 函数选项，新建一个**与函数名同名**的函数m文件（事实上，函数m文件和脚本m文件没有实质性的不同，但是新建函数m文件时将会自动生成函数结构）。这时我们可以看到MATLAB已经自动为我们生成好了函数的结构。

``` matlab
function [ output_args ] = Untitled1( input_args )
%UNTITLED1 此处显示有关此函数的摘要
%   此处显示详细说明


end
```

我们将其更改为下面内容：

``` matlab
function sum = add(num1, num2)
%add 给定两个数，返回其和
%   给定输入的两个数num1和num2，计算得到两数之和并返回（虽然看起来很啰嗦，但是把输入输出讲清楚非常重要）
	sum = num1 + num2;
end
```

通过这个例子我们可以看到，与一般的编程语言不同，MATLAB没有显式的return语句返回计算结果，而是通过在语句中**给输出变量赋值的方法实现返回值**。类似的，如果我们的函数具有多个返回值，我们即需要对所有的输出变量进行赋值。

MATLAB也支持没有任何输出的函数，此时函数仅需要包含如下部分：

``` matlab
% 没有返回值的函数
function function_name(para1, para2, paraN)
	statements
end
```

一般而言，这种没有返回值的函数使用较少，主要是为了实现一些附加作用而使用的。

## 函数调用

定义函数是为了方便我们进行调用。MATLAB中调用已定义的函数非常简单，实际上我们之前已经调用过系统内置函数，例如`zeros()`、`ones()`等。调用函数的方法如下所示：

``` matlab
% 调用函数
[out1, out2, outN] = function_name(para1, para2, paraN)
```

与上述我们定义的add函数相匹配的函数调用为

``` matlab
>> a = add(1, 2)

a =

     3
```

这与我们定义函数的形式非常一致。需要注意的是，当输出变量的个数小于函数实际返回的变量个数时，MATLAB将默认从前到后进行返回，并不会引起报错。下面的例子很好的说明了这一点。

``` matlab
%%% 文件calculate.m %%%

% 计算两个数的四则运算结果
function [res1, res2, res3, res4] = calculate(num1, num2)
	res1 = num1 + num2;
    res2 = num1 - num2;
    res3 = num1 * num2;
    res4 = num1 / num2;
end

%%% 文件Script5_1.m %%%

sum = calculate(1, 2)
[sum, diff, product, quot] = calculate(1, 2)

% 以下是运行Script5_1.m的命令行输出结果
% 注：此为第1行运行结果
sum =
     3
% 注：此为第2行运行结果
sum =
     3
diff =
    -1
product =
     2
quot =
    0.5000
```

之所以强调是Script5_1.m的运行结果，是因为带有输入参数的函数是无法直接被运行的。但是没有输入参数的函数可以直接被运行，例如下列函数在屏幕上打印出Hello World字样。

``` matlab
% 在屏幕上打印出Hello World
function hello()
	disp('Hello World!');
end

% 以下是命令行输出结果
Hello World!
```

## 函数变量

### 函数作为参数

这一部分可能会使得编程基础稍差的同学感到迷惑，不过耐心读几遍还是可以读懂的。

MATLAB一项令人欣喜的功能是可以将函数作为变量看待，其方式为使用'@'字符。例如我们编写了之前的add.m文件后，使用以下命令：

``` matlab
% 将add函数赋值给var_fun函数
var_fun = @add;
% 使用var_fun函数
var_fun(3, 5)

% 以下是命令行输出结果

ans =

     8
```

我们发现，我们定义的var_fun居然实现了和add一样的功能！这看起来很不可思议，但实际上这正是**函数式编程**的基本概念。甚至，我们还可以将函数作为函数参数进行传递。例如我们定义了一个函数operate，这个函数接受一个操作两个数的函数（例如add）和两个数字，给出计算结果。我们可以这样定义：

``` matlab
%%% 文件add.m %%%

function sum = add(num1, num2)
	sum = num1 + num2;
end

%%% 文件operate.m %%%

function result = operate(my_fun, num1, num2)
	result = my_fun(num1, num2);
end

%%% 文件Script5_2.m %%%

operate(@add, 2, 3)

% 以下是命令行输出结果

ans =

     5
```

我们惊讶地发现，我们将add函数作为参数成功传递给了operate，并给出了正确的计算结果。请认真思考上述代码的执行流程。

### 匿名函数

提起函数式编程，就不得不提到大名鼎鼎的Lambda函数。在MATLAB中，同样对Lambda函数提供了支持。为方便不了解函数式编程的同学，下面将统一称之为“匿名函数”。

首先回想我们使用函数的流程。我们需要确定函数的输入与输出、函数名，新建函数m文件，编写函数体，然后在主程序中对函数进行调用。但很多时候，我们编写的函数可能往往只有一行或更少，这种情况下单独编写一个函数m文件就显得非常麻烦。甚至有些时候，这个函数我们只需要用一次，能不能不取名呢？匿名函数（Lambda函数）就很好地解决了这个问题。

匿名函数的声明如下：

``` matlab
@(para1, para2, paraN)expression
```

匿名函数的形式十分简洁，将整个函数的定义缩减到一行内。例如之前的add函数，我们可以这样进行改写

``` matlab
% 定义add函数的Lambda形式
lambda_add = @(a, b)a + b
% 调用函数
lambda_add(3, 4)

% 以下是命令行输出结果

ans =

     7
```

可以看到Lambda函数保持了和原函数一样的功能。我们将之前的Script5_2.m文件进行Lambda函数改写：

``` matlab
%%% 文件operate.m %%%

function result = operate(my_fun, num1, num2)
	result = my_fun(num1, num2);
end

%%% 文件Script5_2.m %%%

% 利用operate函数和匿名函数进行四则运算
operate(@(a,b)a + b, 2, 3)
operate(@(a,b)a - b, 2, 3)
operate(@(a,b)a * b, 2, 3)
operate(@(a,b)a / b, 2, 3)

% 以下是命令行输出结果
ans =
     5
ans =
    -1
ans =
     6
ans =
    0.6667
```

看到这里，相信大多数人一定一头雾水，不禁想到：这有什么卵用？事实上这还真的用卵用，后面的章节中我们可以看到很多匿名函数的使用，因此希望这一节的内容你能够尽量理解~

## 形参vs实参

学过其他编程语言的同学在对待函数时，往往需要急切地弄清楚什么时候函数传递形参，什么时候传递实参呢？

先上结论：**MATLAB函数永远传递形参**

没有理解的同学可以通过下面的例子感受一下。

假设我现在有一个1*5的矩阵，我现在想让这个矩阵的第一个元素加1，那么有什么办法呢？最直观的想法是这样的：

``` matlab
%%% 文件add_one.m %%%
function add_one(mat)
	mat(1, 1) = mat(1, 1) + 1;
end
```

表面上看来，这个函数对于任何传递进来的矩阵mat，都将第一个元素进行了加1操作。我们对其进行一下测试：

``` matlab
%%% 文件Script5_3.m %%%
mat = [1, 2, 3, 4, 5];
add_one(mat);
mat

% 以下是命令行输出结果

mat =

     1     2     3     4     5
```

我们惊讶地发现，mat的值并没有发生任何变化！事实上，无论我们在函数内对传入参数进行什么操作，都不会影响到参数在主程序中的原本值，这是因为MATLAB使用的是“形参传递”。在这个过程中，实际上在MATLAB内部发生了如下的事情：

1. 主程序调用函数，此时需要传递参数mat=[1,2,3,4,5]
2. 主程序将所有需要传递的参数的值复制了一份，生成了参数副本mat_copy=[1,2,3,4,5]
3. 主程序将计算中的所有变量暂存，进入函数执行模式
4. 函数利用mat_copy进行计算，当函数试图修改mat的值时，实际在修改mat_copy的值，使得mat_copy=[2,2,3,4,5]
5. 函数计算完成，告知主程序计算完成，并将计算结果返回主程序
6. 主程序将暂存变量恢复，mat的值仍为[1,2,3,4,5]

因此，无论我们怎么做，主程序中的值都无法在函数中进行修改。那我们怎样实现上述需求呢？一种方式是可以将mat_copy的值直接返回给主程序，主程序再将mat的值替换为函数返回值。如下：

``` matlab
%%% 文件add_one.m %%%
function mat = add_one(mat)
	mat(1, 1) = mat(1, 1) + 1;
end

%%% 文件Script5_3.m %%%
mat = [1, 2, 3, 4, 5];
mat = add_one(mat)

% 以下是命令行输出结果

mat =

     2     2     3     4     5
```

这样做或许显得不美观，但也不失为一种好方法。

还有一种方法则是利用全局变量的方法。

## 全局变量

在介绍全局变量之前，我们需要先了解一下变量空间。变量空间是指MATLAB计算过程中当前状态可以使用的所有变量。例如在主程序中可以访问在主程序中定义的变量，但是在函数运行过程中就无法使用主程序定义的变量，因此我们说它们属于不同的变量空间。

一般而言，MATLAB的变量空间可以有多个，其中所有的脚本m文件共享一个变量空间，例如先执行Script2_1.m，再执行Script2_2.m时仍然可以访问在之前的脚本中已经定义的函数；而不同的函数m文件属于不同的变量空间，且不同于脚本m文件的变量空间，因此任何函数都不能调用在其他函数中或者主程序中定义的变量。

![](https://raw.githubusercontent.com/ghh3809/ghh3809.github.io/master/_posts/_pic/20180410_variable_spaces.jpg)

之所以传递到函数中的参数不能修改，是因为主程序中传进去的参数是属于主程序的变量空间，当函数开始执行时，主程序的变量就被暂存起来无法改动了。那我们自然想到，能不能利用什么方法，使得一个变量不只是属于主程序呢？答案就是全局变量。全局变量不属于任何一个程序模块，在任何时候都可以被修改。

我们使用如下的方式定义或使用一个全局变量：

``` matlab
global global_var;
statements
```

使用任何全局变量前，都要先进行声明，以告知编译器：这是一个全局变量，请不要在局部变量里面寻找。使用全局变量可以对上述的add_one方法进行改写：

``` matlab
%%% 文件add_one.m %%%
function add_one()
	global mat;
	mat(1, 1) = mat(1, 1) + 1;
end

%%% 文件Script5_3.m %%%
global mat;
mat = [1, 2, 3, 4, 5];
add_one();
mat

% 以下是命令行输出结果

mat =

     2     2     3     4     5
```

使用全局变量非常方便，但是需要注意的是，使用全局变量与函数化、模块化的概念完全相悖，大量使用全局变量将使得程序的逻辑控制变得复杂，可读性变差。因此建议大家尽量不要使用全局变量，以免生出“一个月前写的程序自己都看不懂”的悲剧。