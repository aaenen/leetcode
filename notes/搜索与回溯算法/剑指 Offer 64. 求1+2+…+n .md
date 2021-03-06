# 剑指 Offer 64. 求1+2+…+n

```text
求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。
```

**示例1：**

```text
输入: n = 3
输出: 6
```

**示例2：**

```text
输入: n = 9
输出: 45
```

```java
class Solution {
    //利用这一特性，我们可以将判断是否为递归的出口看作 A && B 表达式中的 A 部分，递归的主体函数看作 B 部分。如果不是递归出口，则返回 \textit{True}True，并继续执行表达式 B 的部分，否则递归结束。当然，你也可以用逻辑运算符 || 给出类似的实现，这里我们只提供结合逻辑运算符 && 的递归实现。
    //boolean x = n > 1 为递归的出口
    public int sumNums(int n) {
        boolean x = n > 1 && (n += sumNums(n - 1)) > 0;
        return n;
    }
}
```