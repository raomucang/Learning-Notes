# 二维数组中的查找

## 题目

在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 题解

```java
public class test {
    public static void main(String[] args) {
        int[][] arr = {{1, 2, 8, 9}, {2, 4, 9, 12}, {4, 7, 10, 13}, {6, 8, 11, 15}};
        int number = 7;

        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr[i].length; j++) {
                System.out.print(arr[i][j] + "\t");
            }
            System.out.println();
        }
        System.out.println("number: " + number);

        boolean b = isExist(arr, number);
        System.out.println("是否存在:" + b);
    }

    public static boolean isExist(int[][] arr, int number) {
        int line = 0;
        int column = arr[0].length - 1;
        while (line < arr.length && column >= 0) {
            if (number == arr[line][column]) {
                return true;
            } else if (number < arr[line][column]) {
                column--;
            } else if (number > arr[line][column]) {
                line++;
            }
        }
        return false;
    }
}
```
