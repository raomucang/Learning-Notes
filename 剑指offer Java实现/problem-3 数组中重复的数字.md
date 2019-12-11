# 数组中重复的数字


## 题目一：找出数组中重复的数字

在一个长度为n的数组里的所有数组都在0~n-1的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。例如,如果输入长度为7的数组{2，3，1，0，2，5，3}，那么对应的输出是重复数字2或者3.

## 题一解

```java
public class test {
    public static void main(String[] args) {
        int[] arr = new int[]{2, 3, 1, 0, 2, 5, 3};
        int number = findRepeat(arr);
        System.out.println("重复的数字是:" + number);
    }

    public static int findRepeat(int[] arr) {
        int number = 0;
        int num;
        for (int i = 0; i < arr.length; ) {
            if (i != arr[i]) {
                if (arr[i] == arr[arr[i]]) {
                    return arr[i];
                } else {
                    num = arr[i];
                    arr[i] = arr[num];
                    arr[num] = num;
                }
            } else {
                i++;
            }
        }
        return number;
    }
}
```

## 题目二：不修改数组找出重复的数字

在一个长度为n+1的数组里的所有数字都在1~n的范围内，所以数组中至少有一个数字是重复的。请找出数组中任意一个重复的数字，但不能修改输入的数组。例如，如果输入长度为8的数组{2，3，5，4，3，2，6，7}，那么对应的输出是重复的数字2或者3。

## 题二解

```java
public class test {
    public static void main(String[] args) {
        int[] arr = new int[]{2, 3, 5, 4, 3, 2, 6, 7};
        int number = findRepeat(arr);
        System.out.println("重复的数字是:" + number);
    }

    public static int findRepeat(int[] arr) {
        int start = 1;
        int end = arr.length - 1;
        while (start != end) {
            int count = 0;
            int middle = (start + end) / 2;
            for (int i = 0; i < arr.length; i++) {
                if (arr[i] >= start && arr[i] <= middle) {
                    count++;
                }
            }
            if (count > (middle - start + 1)) {
                end = middle;
            } else {
                start = middle + 1;
            }
        }
        return start;
    }
}

```
