
#Data-Structure #Queue

1. 操作受限的线性表数据结构，先进先出
   1. 入队 enqueue()
   2. 出队 dequeue()
2. 顺序队列和链式队列
3. 循环队列；确定好队空和队满的判定条件。
   1. 队满：`(tail+1)%n = head`
   2. 循环队列会浪费一个数组的存储空间
   3. `tail = (tail+1)%n`
   4. `head = (head+1)%n`
4. 阻塞队列
   1. 队列为空时，阻塞`dequeue`,直到有数据
   2. 队列为满时，阻塞`enqueue`,直到队列有空间
   3. 生产者-消费者模型
5. 并发队列-线程安全的队列。直接在`enqueue`和`dequeue`方法加锁。
6. 无界队列`unbounded queue`,无限排队的队列，导致过多的请求排队等待。基于链表。
7. 相关题目：225，239，347，1047，

题目 225 用队列实现栈：请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（push、top、pop 和 empty）。

实现 MyStack 类：

- void push(int x) 将元素 x 压入栈顶。
- int pop() 移除并返回栈顶元素。
- int top() 返回栈顶元素。
- boolean empty() 如果栈是空的，返回 true ；否则，返回 false 。

```c#
private Queue<int> q1;
    private Queue<int> q2;
    public MyStack()
    {
        q1 = new Queue<int>();
        q2 = new Queue<int>();
    }

    public void Push(int x)
    {
        while (q1.Any()) q2.Enqueue(q1.Dequeue());
        q1.Enqueue(x);
        while (q2.Any()) q1.Enqueue(q2.Dequeue());
    }

    public int Pop()
    {
        return q1.Dequeue();
    }

    public int Top()
    {
        return q1.Peek();
    }

    public bool Empty()
    {
        return !q1.Any();
    }
```
