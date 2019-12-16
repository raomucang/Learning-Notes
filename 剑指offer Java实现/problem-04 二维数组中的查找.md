# 二维数组中的查找

## 题目

在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 题解

这道题的解题思路是从二维数组的右上角开始查找

```java
public class Solution {
    public boolean Find(int target, int [][] array) {
        /*如果数组为空，直接返回 false*/
        if (array == null) {
            return false;
        }

        /*定义查找的初始位置，我们从二维数组的右上角开始查找*/
        int row = 0;
        int col = array[0].length - 1;

        /*到查询的位置的行大于数组的总行数或者查询的位置的列小于0时，则跳出循环，这时已经查询完了整个数组*/
        while (row < array.length && col >= 0) {
            //查询到，返回true
            if (target == array[row][col]) {
                return true;
            /*首先是从右往左进行列查询，当要查询的数字大于查询位置的值时，说明数字不在这一列，查询下一列，当要查询位置小于数字时，则锁定了数字可能存在的列，就需要往下开始查询*/
            } else if (target < array[row][col]) {
                col--;
            /*开始向下查询*/
            }else {
                row++;
            }
        }
        /*查询完整个数字都没有找到，则返回false*/
        return false;
    }
}
```
