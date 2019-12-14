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
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.add(stack1.pop());
            }
        }
        return stack2.pop();
    }
}
```
