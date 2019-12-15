# 打印从 1 到最大的 n 位数

## 题目

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则输出 1、2、3 一直到最大的 3 位数 999。

```java
public class test {
    public static void main(String[] args) {
        int n = 5;

        int[] data = new int[n];

        for (int i = 0; i < 10; i++) {
            data[0] = i;
            printMethod(data, n, 0);
        }
    }

    public static void printMethod(int[] data, int n, int index) {
       if (index == n - 1) {
           printNum(data);
           return;
       }

        for (int i = 0; i < 10; i++) {
            data[index + 1] = i;
            printMethod(data, n, index + 1);
        }
    }

    private static void printNum(int[] data) {
        int notZero = 0;
        boolean isEnd = false;
        for (int i = 0; i < data.length; i++) {
            if (data[i] != 0) {
                notZero = i;
                break;
            }
            if (i == data.length - 1) {
                isEnd = true;
            }
        }
        if (isEnd) {

        } else {
            for (int i = notZero; i < data.length; i++) {
                System.out.print(data[i]);
            }
            System.out.println();
        }
    }
}
```
