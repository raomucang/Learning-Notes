# 旋转数组的最小数字

## 题目

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。<br/>
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

## 题解

这道题考察的是二分法

```java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        //如果数组长度为 0，直接返回 0
        if (array.length == 0) {
            return 0;
        }

        //如果数组旋转了 0 次,或者数组长度只有 1，那么直接返回数组第一个数字
        if (array[0] < array[array.length - 1] || array.length == 1) {
            return array[0];
        }

        int index1 = 0;
        int index2 = array.length - 1;
        int mid = index1;

        //如果前面的数字比后面的小，那么说明找到了最小的数字
        while (array[index1] >= array[index2]) {
            //当两个 index 相距为 1 时，说明后面的数字就是最小的数字，跳出循环
            if (index2 - index1 ==1) {
                mid = index2;
                break;
            }

            mid = (index1 + index2) / 2;

            //如果前面的中间的数大于前面的数，继续缩小范围
            if (array[mid] >= array[index1]) {
                index1 = mid;
                //如果中间的数小于后面的数，继续缩小范围
            } else if (array[mid] <= array[index2]) {
                index2 = mid;
            }
        }
        return array[mid];
    }
}
```
