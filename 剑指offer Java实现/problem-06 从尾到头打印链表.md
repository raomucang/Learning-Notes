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
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = printList(listNode);
        return list;
    }

    public ArrayList<Integer> printList(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        while (listNode != null) {
            list.add(0, listNode.val);
            listNode = listNode.next;
        }
        return list;
    }
}
```

## 题解二，递归方法

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
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        listAdd(list, listNode);
        return list;
    }

    public void listAdd(ArrayList list, ListNode listNode) {
        if (listNode != null) {
            listAdd(list, listNode.next);
            list.add(listNode.val);
        }
    }
}
```
