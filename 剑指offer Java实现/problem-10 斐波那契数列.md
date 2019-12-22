# 斐波那契数列

## 题目一：求斐波那契数列的第 n 项

写一个函数，输入 n，求斐波那契数列的第 n 项。

## 题一解

递归方法（代码简洁，但是效率不高）：

```java
public class Solution {
    public int Fibonacci(int n) {
        if (n <= 0) {
            return 0;
        }

        if (n == 1) {
            return 1;
        }

        //斐波那契数列的第 n 项等于 n - 1 项加上 n - 2 项
        return Fibonacci(n - 1) + Fibonacci(n - 2);
    }
}
```

实用解法：避免了递归中的重复运算

```java
public class Solution {
    public int Fibonacci(int n) {
        if (n < 2) {
            return n;
        }

        int[] arr = new int[n + 1];
        arr[0] = 0;
        arr[1] = 1;


        for (int i = 2; i <= n; i++) {
            arr[i] = arr[i - 1] + arr[i - 2];
        }

        return arr[n];
    }
}
```

## 题目二：青蛙跳台阶问题

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

## 题二解

```java
public class Solution {
    public int JumpFloor(int target) {
        if (target == 1) {
            return 1;
        }

        if (target == 2) {
            return 2;
        }

        return JumpFloor(target - 1) + JumpFloor(target - 2);
    }
}
```

## 题目三：变态跳台阶

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

## 题三解

```math

f(n) = 0, (n = 1)
f(n) = 1, (n = 1)
f(n) = 2 * f(n - 1), (n >= 2)
```

```java
public class Solution {
    public int JumpFloorII(int target) {
        if (target <= 0) {
            return 1;
        }

        if (target == 1) {
            return 1;
        }
        return 2 * JumpFloorII(target - 1);
    }
}
```
