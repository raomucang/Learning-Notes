# 二进制中 1 的个数

## 题目

请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

## 题解

```java
public class Solution {
    public int NumberOf1(int n) {
        int count = 0;

        while (n != 0) {
            count ++;
            n = n & (n - 1);
        }
        return count;
    }
}
```
