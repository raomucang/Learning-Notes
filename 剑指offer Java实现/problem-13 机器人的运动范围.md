# 机器人的运动范围

## 题目

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

## 题解

```java
public class Solution {
    public int movingCount(int threshold, int rows, int cols)
    {
        boolean[] flag = new boolean[rows * cols];
        return movingCount(threshold, rows, cols, flag, 0, 0);
    }

    private int movingCount(int threshold, int rows, int cols, boolean[] flag, int row, int col) {
        if (row < 0 || col < 0 || row >= rows || col >= cols || numSum(row) + numSum(col) > threshold || flag[cols * row + col] == true) {
            return 0;
        }

        flag[row * cols + col] = true;

        return movingCount(threshold, rows, cols, flag, row + 1, col) +
               movingCount(threshold, rows, cols, flag, row - 1, col) +
               movingCount(threshold, rows, cols, flag, row, col + 1) +
               movingCount(threshold, rows, cols, flag, row, col - 1) + 1;
    }

    private int numSum(int i) {
        int a= 0;
        do{
            a+= i%10;
        }while((i = i/10) > 0);
        return a;
    }
}
```
