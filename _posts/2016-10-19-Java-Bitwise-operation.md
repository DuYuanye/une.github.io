---
layout: post
title: Java位操作及应用
description: "Java位操作的简单介绍及具体位操作使用场景总结(持续更新)"
modified: 2016-10-19
tags: [Java,位运算,位操作,位操作应用]
image:
  background: triangular.png
---

### 1.概念：

>​    In digital computer programming, a bitwise operation operates on one or more bit patterns or binary numerals at the level of their individual bits. It is a fast, simple action directly supported by the processor, and is used to manipulate values for comparisons and calculations.
>​    On simple low-cost processors, typically, bitwise operations are substantially faster than division, several times faster than multiplication, and sometimes significantly faster than addition. While modern processors usually perform addition and multiplication just as fast as bitwise operations due to their longer instruction pipelines and other architectural design choices, bitwise operations do commonly use less power because of the reduced use of resources.[(维基百科)](https://en.wikipedia.org/wiki/Bitwise_operation)



>​    **位操作**是程序设计中对[位模式](https://zh.wikipedia.org/w/index.php?title=%E4%BD%8D%E6%A8%A1%E5%BC%8F&action=edit&redlink=1)按位或[二进制数](https://zh.wikipedia.org/wiki/%E4%BA%8C%E9%80%B2%E4%BD%8D%E6%95%B8)的一元和二元操作。在许多古老的[微处理器](https://zh.wikipedia.org/wiki/%E5%BE%AE%E5%A4%84%E7%90%86%E5%99%A8)上，位运算比加减运算略快，通常位运算比乘除法运算要快很多。在现代架构中，情况并非如此：位运算的运算速度通常与加法运算相同（仍然快于乘法运算）。

### 2.操作对象：

>​    Java位运算是针对于**整型类型**的二进制进行的移位操作  
>​    Java整型数据类型有：byte、char、short、int、long  
>   **位操作操作的是补码！补码！补码！**(重要的事情说三遍)   

要把他们转换成二进制形式，必须首先知道他们各占多少字节：

|  数据类型   | 所占位数 |
| :-----: | :--: |
|  byte   |  8   |
| boolean |  8   |
|  short  |  16  |
|   int   |  32  |
|  long   |  64  |
|  float  |  32  |
| double  |  64  |
|  char   |  16  |

### 3.操作符
>​    Java位操作包括**位操作符**和**移位操作**  
>​    位操作符包括：**&(位与) , |(位或) , ^(位异或) , ~(位非)**   
>​    位移操作包括：**\<\<(左移) , \>\>(右移) , \>\>\>(无符号右移)**

下面简单举例说明一下各种位操作

#### 3.1  &(位与)

```
4&6
0000 0000 0000 0000 0000 0000 0000 0100  
0000 0000 0000 0000 0000 0000 0000 0110  
0000 0000 0000 0000 0000 0000 0000 0100  
结果：4  
```

再看负数的情况：  

```
-1&6  
1000 0000 0000 0000 0000 0000 0000 0001  //-1的原码  
1111 1111 1111 1111 1111 1111 1111 1110  //-1的反码  
1111 1111 1111 1111 1111 1111 1111 1111  //-1的补码  
0000 0000 0000 0000 0000 0000 0000 0110  //6的补码(正数的补码，反码和原码相同)  
0000 0000 0000 0000 0000 0000 0000 0110  //计算后的补码(首位为0则是正数,原码与补码相同)  
结果：6
```

#### 3.2  |(位或) 

```
4|6  
0000 0000 0000 0000 0000 0000 0000 0100  
0000 0000 0000 0000 0000 0000 0000 0110  
0000 0000 0000 0000 0000 0000 0000 0110  
结果：6  
```

#### 3.3  ^(位异或)

```
4^6  
0000 0000 0000 0000 0000 0000 0000 0100  
0000 0000 0000 0000 0000 0000 0000 0110  
0000 0000 0000 0000 0000 0000 0000 0010  
结果：2
```

#### 3.4  ~(位非)

```
~4  
0000 0000 0000 0000 0000 0000 0000 0100  
1111 1111 1111 1111 1111 1111 1111 1011  //符号位为1，表示负数，所以应该计算原码  
1111 1111 1111 1111 1111 1111 1111 1010  //-1获取反码  
1000 0000 0000 0000 0000 0000 0000 0101  //原码  
结果：-5
```

#### 3.5  \<\<(左移)
**位移运算如果位移超过该数据类型表示范围，则先取模，再进行运算(比如针对int型，4\>\>32等价于4\>\>0)**

```
2 >> 2  
0000 0000 0000 0000 0000 0000 0000 0010  
0000 0000 0000 0000 0000 0000 0000 1000   //左移两位  
结果：8
```

#### 3.6   \>\>(右移)
**若正数,高位补0,负数,高位补1**

```
4 >> 2  
0000 0000 0000 0000 0000 0000 0000 0100  
0000 0000 0000 0000 0000 0000 0000 0001  
结果：1
```

#### 3.7  \>\>\>(无符号右移)
**不论正负,高位均补0**

```
-1 >>> 1  
1111 1111 1111 1111 1111 1111 1111 1111  //-1补码  
0111 1111 1111 1111 1111 1111 1111 1111  
结果：2147483647 
```

### 4.应用场景

* 判断int型变量a是奇数还是偶数

```
 a&1 = 0 偶数
 a&1 = 1 奇数 
```

* 求平均值,如果x+y的和会超过int型的值的范围，就可以使用位运算来解决

```
 公式为 (x&y)+((x^y)>>1) 
 下面简单解释一下：  
 在二进制进行计算时，x和y的二进制的对应位都有2种情况(相同，不同)。
 相同情况下0+0还是0，不用管。1+1需要向前进一位，但是最后会除以2，所以还是原来位置。
 不同情况1+0计算结果为1，最后除以2需要右移一位。
```


* 对于一个大于0的整数，判断它是不是2的n次方

```
   ((x&(x-1))==0)&&(x!=0)
```


* 有两个int类型变量x、y,要求两者数字交换

```
 x ^= y;   
 y ^= x;   
 x ^= y; 
```

* 求绝对值

```
 int abs( int x )   
 {   
   int y ;   
   y = x >> 31 ;   
   return (x^y)-y ; //or: (x+y)^y   
 }  
 //如果x为负，其实结果就是对负数求负
```
[看不明白的点这里](https://my.oschina.net/yumifan/blog/221890)

* 取模运算

```
 a % (2^n) 等价于 a & (2^n - 1)   
 //这个在HashMap的原码中用到了，根据key的hash值插入桶中的时候 
```

* 求相反数

```
 ~x+1
```

没有啦，以后遇到一些比较好的位操作使用场景会再来更新  
如果发现有什么问题，请联系我更改  

参考：  
[Java位运算总结：位运算用途广泛](http://www.52ij.com/jishu/102.html)  
[Java基础——原码, 反码, 补码 详解](http://www.linuxidc.com/Linux/2015-02/113863.htm)  
[wikipedia.Bitwise_operation](https://en.wikipedia.org/wiki/Bitwise_operation)  
[java基本位操作 ](http://blog.sina.com.cn/s/blog_4df91b180100uim5.html)  
[绝对值函数(位操作法)](https://my.oschina.net/yumifan/blog/221890)  