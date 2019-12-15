# 删除链表的节点

## 题目一：在 O(1) 时间内删除链表节点

给定单向链表的头指针和一个节点指针，定义一个函数在 O(1) 时间内删除该节点。链表节点与函数的定义如下：

```c++
struct ListNode
{
    int m_nValue;
    ListNode* m_pNext;
};

void DeleteNode(ListNode** pListHead, ListNode* pToBeDeleted);
```

## 题一解

[本题解来自 CSDN 白夜行515](https://blog.csdn.net/baiye_xing/article/details/78428561)

```java
public void deleteNode(ListNode head, ListNode deListNode) {
        if (deListNode == null || head == null)
            return;

        if (head == deListNode) {
            head = null;
        } else {
            // 若删除节点是末尾节点，往后移一个
            if (deListNode.nextNode == null) {
                ListNode pointListNode = head;
                while (pointListNode.nextNode.nextNode != null) {
                    pointListNode = pointListNode.nextNode;
                }
                pointListNode.nextNode = null;
            } else {
                deListNode.data = deListNode.nextNode.data;
                deListNode.nextNode = deListNode.nextNode.nextNode;
            }
        }
    }
```

## 题二

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

## 题二解

```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ListNode deleteDuplication(ListNode pHead)
    {
        if (pHead == null || pHead.next == null) {
            return pHead;
        }

        ListNode temp = pHead;
        ListNode index = new ListNode(0);
        index.next = pHead;
        ListNode result = index;
        while (temp != null) {
            if (temp.next != null && temp.val == temp.next.val) {
                while (temp.next != null && temp.val == temp.next.val) {
                    temp = temp.next;
                }
                temp = temp.next;
                index.next = temp;
            } else {
                index = index.next;
                temp = temp.next;
            }
        }
        return result.next;
    }
}
```
