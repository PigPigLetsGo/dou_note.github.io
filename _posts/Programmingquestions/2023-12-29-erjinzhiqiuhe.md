---
layout: post
title: 二进制求和
categories: [java,算法,编程题]
description: 二进制求和的题解
keywords: 编程题
---

# 二进制求和

给你两个二进制字符串a和b,以二进制字符串的形式返回它们的和

示例:

>  示例1:
>
>  ```
>  输入:a = "11", b = "1"
>  输出："100"
>  ```
>
>  示例2:
>
>  ```
>  输入：a = "1010", b = "1011"
>  输出："10101"
>  ```

---
提示:

-  `1 <= a.length, b.length <= 104`
-  `a` 和 `b` 仅由字符 `'0'` 或 `'1'` 组成
-  字符串如果不是 `"0"` ，就不含前导零

==思路==:

1.  判断如果字符串a为null或者长度为0那么就返回b字符串因为0+1=1,相反字符串b同理
2.  创建StringBuilder对象,用于追加字符串拼接的结果,定义两个字符串的长度[^长度减-防止IndexOutOfBountException]防止一下角标越界
3.  定义int类型临时变量c
4.  while循环i大于等于0或者j大于等于0则为true,执行方法体,if判断如果i大于等于0则执行临时变量c加等于`a.charAt(i --) - '0';`将字符串类型转换为int数字,然后加等于到c中,第二个if判断j是否大于等于0操作同 i
5.  StringBuilder对象调用append追加字符串拼接(c % 2);因为二进制==逢二进一==所以要取模2如果为2那么取模结果为0
6.  最后将c临时变量>>=按位右移1位==(1或2按位右移后都为0)==。

```java
public String addBinary(String a, String b) {
        if(a == null || a.length() == 0) 
           return b;
        if(b == null || b.length() == 0) 
           return a;
        StringBuilder stb = new StringBuilder();
        int i = a.length() - 1;
        int j = b.length() - 1;
        int c = 0;  // 进位
        while(i >= 0 || j >= 0) {
            if(i >= 0) c += a.charAt(i --) - '0';
            if(j >= 0) c += b.charAt(j --) - '0';
            stb.append(c % 2);
            c >>= 1;
        }
        String res = stb.reverse().toString();
        return c > 0 ? '1' + res : res;
}
```

