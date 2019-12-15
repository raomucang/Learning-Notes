# 数值的整数次方

## 问题

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。
保证base和exponent不同时为0

## 题解

```java
public class Solution {
    public double Power(double base, int exponent) {
        double temp = 1;

        if (exponent > 0) {
            for (int i = 1; i <= exponent; i++) {
                temp = temp * base;
            }
            return temp;
        }
        else if (exponent < 0) {
            exponent = -exponent;

            for (int i = 1; i <= exponent; i++) {
                temp = temp * base;
            }
            return 1.0/temp;
        } else {
            return 1;
        }
  }
}
```
