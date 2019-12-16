# 从尾到头打印链表

## 题目

输入一个链表的头节点，从尾到头反过来打印出每个节点的值。链表节点定义如下：

```c++
struct ListNode
{
    int m_nKey;
    ListNode* m_pNext;
};
```

## 题解一，栈方法

使用栈，先进后出

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/

/*代码简单，不需要注释*/
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> array = new ArrayList<Integer>();

        while (listNode != null) {
            array.add(0, listNode.val);
            listNode = listNode.next;
        }

        return array;
    }
}
```

## 题解二，递归方法

递归方案，不断递归直到递归到链表末尾，再从链表末尾开始将值加入到集合中

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/

/*这个代码也很简单，也不需要注释。。。😓*/
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> array = new ArrayList<Integer>();
        arrayAdd(array, listNode);
        return array;
    }

    public void arrayAdd(ArrayList array, ListNode listNode) {
        if (listNode != null) {
            arrayAdd(array, listNode.next);
            array.add(listNode.val);
        }
    }

}
```
