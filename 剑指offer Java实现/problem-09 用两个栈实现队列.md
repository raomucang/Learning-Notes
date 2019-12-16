# 用两个栈实现队列

## 问题

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead，分别完成在队列尾部插入节点和在队列头部删除节点的功能。

```c++
template<typename T> class CQueue
{
    public:
        CQueue(void);
        ~CQueue(void);

        void appendTail(const T & node);
        T deleteHead();

    private:
    stack<T> stack1;
    stack<T> stack1;
}
```

## 题解

```java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        stack1.push(node);
    }

    public int pop() {
        /*如果 statck2 不为空，说明还有 node 存储在stack2 中，如果这时将 stack1 的node 取出放入 stack2，原先 stack2 中的顺序靠前的 node 就会排到后面，就会出错*/
        if (stack2.isEmpty()) {
            /*将 stack1 中的 node 出栈，依次压入 stack2，最先压入 stack1 的 node 就变成了stack2最后压入栈的*/
            while (!stack1.isEmpty()) {
                stack2.add(stack1.pop());
            }
        }
        return stack2.pop();
    }
}
```
