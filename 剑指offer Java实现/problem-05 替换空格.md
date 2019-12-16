# 替换空格

## 题目

请实现一个函数，把字符串中的每个空格替换成"%20"。例如，输入"We are happy."，则输出"We%20are%20happy."。

## 题解

本题需要考虑：
1.替换后的字符串比原字符串长
2.从后往前赋值会简便一些

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        int oldLength = str.length();
        int newLength = oldLength;

        /*遍历原字符串，如果有空格那么新字符串的长度需要增长两个位置*/
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == ' ') {
                newLength = newLength + 2;
            }
        }

        /*给字符串设定新长度*/
        str.setLength(newLength);

        int oldIndex = oldLength - 1;
        int newIndex = newLength - 1;

        /*重新遍历字符串，从后往前给字符串赋值*/
        while(newIndex >= 0) {
            if (str.charAt(oldIndex) == ' ') {
                str.setCharAt(newIndex--, '0');
                str.setCharAt(newIndex--, '2');
                str.setCharAt(newIndex--, '%');
                oldIndex--;
            } else {
                str.setCharAt(newIndex--, str.charAt(oldIndex));
                oldIndex--;
            }
        }

        /*返回值需要是 String 类型*/
        return str.toString();
    }
}
```
