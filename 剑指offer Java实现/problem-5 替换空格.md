# 替换空格

## 题目

请实现一个函数，把字符串中的每个空格替换成"%20"。例如，输入"We are happy."，则输出"We%20are%20happy."。

## 题解

```java
public class test {
    public static void main(String[] args) {
        StringBuffer str = new StringBuffer("We are happy.");
        str = replaceSpace(str);
        System.out.println(str);
    }

    public static StringBuffer replaceSpace(StringBuffer str) {
        int oldLength = str.length();
        int newLength = 0;
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == ' ') {
                newLength = newLength + 2;
            }
        }
        newLength = newLength + oldLength;
        int oldIndex = oldLength - 1;
        int newIndex = newLength - 1;
        str.setLength(newLength);

        while (oldIndex >= 0) {
            if (str.charAt(oldIndex) == ' ') {
                str.setCharAt(newIndex--, '0');
                str.setCharAt(newIndex--, '2');
                str.setCharAt(newIndex--, '%');
            } else {
                str.setCharAt(newIndex--, str.charAt(oldIndex));
            }
            oldIndex--;
        }
        return str;
    }
}

```
