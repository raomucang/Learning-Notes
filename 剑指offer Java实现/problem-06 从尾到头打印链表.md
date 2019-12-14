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
import java.util.ArrayList;

public class test {
    public static void main(String[] args) {
        //准备链表
        ListNode listNode1 = new ListNode(1);
        ListNode listNode2 = new ListNode(2);
        ListNode listNode3 = new ListNode(3);
        listNode1.next = listNode2;
        listNode2.next = listNode3;

        ArrayList<Integer> list = printListFromTailToHead(listNode1);
        System.out.println(list);

    }

    public static ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        while (listNode != null) {
            list.add(0, listNode.val);
            listNode = listNode.next;
        }
        return list;
    }
}

//链表节点
class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}

```

## 题解二，递归方法

```java
import java.util.ArrayList;

public class test {
    public static void main(String[] args) {
        //准备链表
        ListNode listNode1 = new ListNode(1);
        ListNode listNode2 = new ListNode(2);
        ListNode listNode3 = new ListNode(3);
        listNode1.next = listNode2;
        listNode2.next = listNode3;

        ArrayList<Integer> list = printListFromTailToHead(listNode1);
        System.out.println(list);

    }

    static ArrayList<Integer> list = new ArrayList<Integer>();
    public static ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if (listNode != null) {
            //listNode = listNode.next;
            printListFromTailToHead(listNode.next);
            list.add(listNode.val);
        }
        return list;
    }
}

//链表节点
class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}

```
