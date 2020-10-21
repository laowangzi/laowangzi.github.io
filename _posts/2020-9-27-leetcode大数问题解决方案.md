---
published: true
title：leetcode大数问题解决方案
category: algorithm
tags: 
  - leetcode
  - algorithm
layout: post

---
#leetcode大数问题解决方案
在leetcode刷题过程中，常见需求当结果大于1000000007时需要取模输出。下面记录解决方法。
##Java int类型取值范围
Integer类取值和 int 类型取值一致，取值范围是从-2147483648 （10位数）至 2147483647（-231至 231-1） ，包括-2147483648 和 2147483647。  
但是**对于Integer类java为了提高效率，初始化了-128--127之间的整数对象，因此Integer类取值-128--127的时候效率最高。**
##剑指offer_10 斐波那契数列
本题的思路属于，对每个子结果求模来保证总体结果符合要求。这是因为最终结果是由子结果由加法得到的。
##剑指offer_14.2 剪绳子2
要求剪短绳子各段乘积达到最大值并且结果需要与1000000007取模。本题第一道通过数学推导解答。

##循环求余法
此方法的数学推导：

    由于xa⊙p=[(xa−1⊙p)(x⊙p)]⊙p=[(xa−1⊙p)x]⊙p
    ⊙代表求余运算
    所以可得        
        int res=1;
        for(int i=0;i<a;i++)
        {
            res=(res*3)%1000000007;
        }
        return res;
本方法时间复杂度为n。这个方法解决问题的思路和上题中类似，压着数字不让涨上来。
##快速幂求余
思路也很简单，x平方一次求一次余，这样可以把时间复杂度降到o(lgn),但是也增加了一个限制条件就是需要数字类型可以存储得下x^2
   
    def remainder(x, a, p):
    rem = 1
    while a > 0:
        if a % 2: rem = (rem * x) % p  #注意此处
        x = x ** 2 % p
        a //= 2
    return rem
    作者：jyd
    链接：https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/solution/mian-shi-ti-14-ii-jian-sheng-zi-iitan-xin-er-fen-f/


