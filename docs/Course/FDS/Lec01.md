# 算法分析和抽象数据类型

## Perface

* clock() ：捕捉从程序开始运行到 clock() 被调用所耗费的时间。
* 这个时间单位是 clock tick ，即”时钟打点“。
* 常数 CLK_TCK ：机器时钟每秒所走的时钟打点数。
??? note
      ```c 
            clock_t start, stop;
            /* clock_t 是 clock()函数返回的变量类型 */
            double duration;
            /* 记录被测函数运行时间，以秒为单位 */
            int main()
            {
                /* 不在测试范围内的准备工作放在clock()调用之前 */
                start = clock();	/* 开始计时 */
                MyFunction();
                stop = clock();
                duration = ((double)(stop - start))/CLK_TCK;
                /* 其他不在测试范围的处理写在后面，例如输出duration的值 */
                return 0;
            }
      ```

## 简介->什么是数据结构

### 数据对象在计算机中的组织方式

#### 逻辑结构

* 一对一的结构，叫做”线性结构“
* 一对多的逻辑结构，叫做”树型结构“
* 多对多的复杂关系网，这个关系网叫做”图“

#### 物理存储结构 (如数组形式、链表形式 ···

!!! note 
      描述数据结构

抽象数据类型 (Abstract Data Type) | ADT

1. 数据类型
      1. 数据对象集
      2. 数据集合相关联的操作集
2. 抽象：描述数据类型的方法不依赖于具体实现
      1. 与存放和数据的机器无关
      2. 与数据存储的物理结构无关
      3. 与实现操作的算法和编程语言均无关

* 只描述数据对象集和相关操作集”是什么“，并不涉及”如何做到“的问题
   * 其中的抽象可以从如下看
       * e.g：ElementType 是通用数据类型 (抽象)，需要 double ，在前面 define 即可
       * e.g：矩阵是用二维数组、一维数组还是十字链表实现的都不重要，只是实现一个矩阵
       * e.g：Matrix Add 中不在乎按行加还是按列加，用什么语言实现都不在乎
   

>  抽象的好处->一方面是提高程序的复用性，另一方面，让我们侧重去了解程序的逻辑结构

---

## 算法分析

### 定义

#### 算法 (Algorithm) 

1. 一个有限指令集
2. 接受一些输入 (有些情况下不需要输入) 
3. 产生输出
4. 一定在有限步骤之后终止
5. 每一条指令必须
      1. 有充分明确的目标，不可以产生歧义
      2. 计算机能处理的范围之内
      3. 描述应不依赖于任何一种计算机语言及具体的实现手段

### 衡量算法的两个指标

#### 空间复杂度 S(n)

根据算法写成的程序在执行时 **占用存储单元的长度** 。这个长度往往与输入数据的规模有关。空间复杂度过高的算法可能导致使用的内存超限，造成程序非正常中断。

#### 时间复杂度 T(n)

根据算法写成的程序在执行时 **耗费时间的长度** 。这个长度往往也与输入数据的规模有关。

### 复杂度的渐进表示法

1. $T(n) =  O  (f(n))$ 表示存在常数 $C>0, n_0 > 0$, 使得当 $n \ge  n_0$ 时有 $T(n) \le C×f(n)$

2. $T(n) =\Omega(g(n))$ 表示存在常数 $C>0，n_0>0$, 使得当 $n \ge n_0$ 时有 $T(n) \ge C×g(n)$

3. $T(n) =\Theta(h(n))$ 表示同时有 $T(n) = O(h(n))$ 和 $T(n) = \Omega(h(n))$

### TIPs

1. 若有两段算法分别有复杂度 $T_1(n) = O(f_1(n))和T_2(n) = O(f_2(n))$，则

      1. $T_1(n) + T_2(n) = max(O(f_1(n)), O(f_2(n)))$ 表示两个算法拼接起来
   
      2. $T_1(n) \times T_2(n) = O(f_1(n) \times f_2(n))$ 表示两个算法嵌套起来
   
2. 若 $T(n)$ 是关于n的k阶多项式，那么 $T(n)= \Theta (n^k)$

3. 一个for循环的时间复杂度等于循环次数乘以循环体代码的复杂度

4. if-else结构的复杂度取决于if的条件判断复杂度和两个分支部分的复杂度，总体复杂度取三者中最大

### Checking Your Analysis

1.  Method 1
    1.  When $T(N) = O(N)$, check if $T(2N)/T(N)\approx 2$ 
   
    2.  When $T(N) = O(N^2)$, check if $T(2N)/T(N)\approx 4$ 
   
    3.  When $T(N) = O(N^3)$, check if $T(2N)/T(N)\approx 8$ 
   
    4.  ···

2.  Method 2
    1.  When $T(N) = O(f(N))$, check if $\lim\limits_{N\rightarrow\infty}\frac{T(N)}{f(N)} \approx Constant$

---
!!! info 
      一个分析复杂度的方法

### 主定理 | Master

#### 介绍

Master定理，又称主定理，用于程序的时间复杂度计算 (常用于递归调用算法的时间复杂度) 

#### 用法

递归算法时间复杂度形如：

1. $T(n) = O(1), n = 1$

2. $T(n) = aT(\frac{n}{b}) + f(n) , n > 1$

其中，$a \ge 1; b > 1 ;$$f(n)$ 表示不参与递归部分的时间复杂度, 规定 $C_{crit}=log_ba$

> 第二条公式表示：将一个规模为n的问题分为 $a$ 个规模为 $\frac{n}{b}$ 子问题，每次递归将带来 $f(n)$ 的额外计算，然后通过对这 $a$ 个子问题的解的综合，得到原问题的解

那么有：

1. 当 $f(n) = O(n^c)$ ，且 $c < C_{crit}$ 时有： $T(n) = \Theta(n^{C_{crit}})$ 
??? Example
      $T(n) = 8T(\frac{n}{2}) + 1000n^2$

      此时 $a=8, b=2, f(n)=1000n^2$

      $c=2<3=log_ba=C_{crit}$

      故 $T(n)=\Theta(n^3)$

2. 当 $f(n)=O(n^c)$ , 且 $c > C_{crit}$ 时有：$T(n)=\Theta(f(n))$

??? Example
      $T(n) = 2T(\frac{n}{2}) + n^2$

      此时 $a=2, b=2, f(n)=n^2$

      $c=2 > 1=log_ba=C_{crit}$

      故 $T(n)=\Theta(n^2)$

3. 当 $f(n)=O(n^c)$ , 且 $c=C_{crit}$ 时：$T(n)=\Theta(n^clog~n)$
   
4. 若存在非负整数 $k$ ，使得 $f(n)=\Theta(n^{C_{crit}}log_kn)$ ，那么 $T(n)=\Theta(n^{C_{crit}}log_{k+1}n)$
  
??? Quote
      1. [算法时间复杂度分析方法 - 代祖华 - 博客园](https://www.cnblogs.com/nwnu-daizh/p/8652285.html)<br>
      2. [Master定理学习笔记 - water_mi - 博客园](https://www.cnblogs.com/water-mi/p/9794604.html)

