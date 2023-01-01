
#Data-Structure #Stack

1. 后进先出，先进后出，操作受限的线性表
2. 数组实现的栈=顺序栈，链表实现的栈=链式栈

   1. 栈的相关 leetcode 题目：20,155,232,844,224,682,496.

### 相关题目

1. 20 题目描述：有效的括号,给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

```c#
 public class Solution {
     public bool IsValid(string s) {
         Stack<char> sc = new Stack<char>();
         Dictionary<char, char> otherChar = new Dictionary<char, char>() { { '(', ')' }, { '[',']' },{ '{','}'} };
         foreach( var item in s)
         {
             if(otherChar.ContainsKey(item)) sc.Push(item);
             else
             {
                 if (sc.Count == 0) return false;
                 char c = sc.Pop();
                 if (otherChar[c] != item) return false;
             }

         }
         return sc.Count == 0;

     }
 }
```

2. 155 题描述：设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

实现 MinStack 类:

- MinStack() 初始化堆栈对象。
- void push(int val) 将元素 val 推入堆栈。
- void pop() 删除堆栈顶部的元素。
- int top() 获取堆栈顶部的元素。
- int getMin() 获取堆栈中的最小元素。

```C#
public class MinStack {

    private int?[] _array;
    private int _size = 0;

    private const int _defaultCapacity = 10;

    public MinStack()
    {
        _array = new int?[_defaultCapacity];
    }

    public void Push(int val)
    {
        if (_array.Length == _size)
        {
            int?[] newArray = new int?[_size * 2];
            Array.Copy(_array, newArray, _size);
            _array = newArray;
        }
        _array[_size++] = val;
    }

    public void Pop()
    {
        _array[--_size] = null;
    }

    public int Top()
    {
        return (int)_array[_size - 1];
    }

    public int GetMin()
    {
        return (int)_array.Min();
    }
}
```

- 占用内存最小的解法

```C#
public class MinStack {

    Stack<int> stack;
    Stack<int> minStack;

    public MinStack()
    {
        stack= new Stack<int>();
        minStack= new Stack<int>();
    }

    public void Push(int val)
    {
        stack.Push(val);
        if(minStack.Count == 0 || minStack.Peek() >= val)
        {
            minStack.Push(val);
        }
    }

    public void Pop()
    {
        if(minStack.Count!= 0 && stack.Pop() == minStack.Peek())
        {
            minStack.Pop();
        }
    }

    public int Top()
    {
        return stack.Peek();
    }

    public int GetMin()
    {
        return minStack.Peek();
    }
}

```

3. 232 题，用栈实现队列。题目描述：请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（push、pop、peek、empty）：

实现 MyQueue 类：

- void push(int x) 将元素 x 推到队列的末尾
- int pop() 从队列的开头移除并返回元素
- int peek() 返回队列开头的元素
- boolean empty() 如果队列为空，返回 true ；否则，返回 false
  说明：

- 你 只能 使用标准的栈操作 —— 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。

```C#
public class MyQueue {

    Stack<int> leftStack;
    Stack<int> rightStack;

    public MyQueue()
    {
        leftStack= new Stack<int>();
        rightStack= new Stack<int>();
    }

    public void Push(int x)
    {
        if(leftStack.Count==0) leftStack.Push(x);
        else
        {
            while(leftStack.Count != 0)
            {
                int v = leftStack.Pop();
                rightStack.Push(v);
            }
            leftStack.Push(x);
            while(rightStack.Count != 0)
            {
                int v = rightStack.Pop();
                leftStack.Push(v);
            }
        }

        //另一种解题思路
        while(leftStack.Count !=0) rightStack.Push(leftStack.Pop());
        leftStack.Push(x);
        while(rightStack.Count != 0) leftStack.Push(rightStack.Pop());
    }

    public int Pop()
    {
        return leftStack.Pop();
    }

    public int Peek()
    {
        return leftStack.Peek();
    }

    public bool Empty()
    {
        return leftStack.Count == 0;
    }
}
```

844 题目：给定 s 和 t 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 true 。# 代表退格字符。

注意：如果对空文本输入退格字符，文本继续为空。
示例 1：

输入：s = "ab#c", t = "ad#c"
输出：true
解释：s 和 t 都会变成 "ac"。
示例 2：

输入：s = "ab##", t = "c#d#"
输出：true
解释：s 和 t 都会变成 ""。
示例 3：

输入：s = "a#c", t = "b"
输出：false
解释：s 会变成 "c"，但 t 仍然是 "b"。

```C#
public class Solution {
    public bool BackspaceCompare(string s, string t) {
        Stack<char> l = new Stack<char>();
        Stack<char> r = new Stack<char>();

        foreach(var item in s)
        {
            if(item == '#'){
                 if(l.Count > 0) l.Pop();
            }
            else l.Push(item);
        }
        foreach(var item in t)
        {
            if (item == '#' ){
                if(r.Count > 0) r.Pop();
            }
            else r.Push(item);
        }
        if (l.Count != r.Count) return false;
        while(l.Count > 0){
            if(l.Pop() != r.Pop()) return false;
        }
        return true ;
    }
}

```

题目 682，棒球比赛：你现在是一场采用特殊赛制棒球比赛的记录员。这场比赛由若干回合组成，过去几回合的得分可能会影响以后几回合的得分。

比赛开始时，记录是空白的。你会得到一个记录操作的字符串列表 ops，其中 ops[i] 是你需要记录的第 i 项操作，ops 遵循下述规则：

整数 x - 表示本回合新获得分数 x
"+" - 表示本回合新获得的得分是前两次得分的总和。题目数据保证记录此操作时前面总是存在两个有效的分数。
"D" - 表示本回合新获得的得分是前一次得分的两倍。题目数据保证记录此操作时前面总是存在一个有效的分数。
"C" - 表示前一次得分无效，将其从记录中移除。题目数据保证记录此操作时前面总是存在一个有效的分数。
请你返回记录中所有得分的总和。

```c#
public int CalPoints(string[] operations) {
        Stack<int> stack = new Stack<int>();
        foreach (var s in operations)
        {
            if (s == "+")
            {
                int num1 = stack.Pop();
                int num2 = stack.Peek();
                stack.Push(num1);
                stack.Push(num1 + num2);
            }
            else if (s == "D")
            {

                int num1 = stack.Peek();
                stack.Push(2 * num1);
            }
            else if (s == "C")
            {

                stack.Pop();
            }
            else stack.Push(int.Parse(s));
        }
        int sum = 0;
        while (stack.Any())
        {
            sum += stack.Pop();
        }
        return sum;
    }
```

题目 496，下一个更大元素：
nums1 中数字 x 的 下一个更大元素 是指 x 在 nums2 中对应位置 右侧 的 第一个 比 x 大的元素。

给你两个 没有重复元素 的数组 nums1 和 nums2 ，下标从 0 开始计数，其中 nums1 是 nums2 的子集。

对于每个 0 <= i < nums1.length ，找出满足 nums1[i] == nums2[j] 的下标 j ，并且在 nums2 确定 nums2[j] 的 下一个更大元素 。如果不存在下一个更大元素，那么本次查询的答案是 -1 。

返回一个长度为 nums1.length 的数组 ans 作为答案，满足 ans[i] 是如上所述的 下一个更大元素 。

示例 1：

输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：

- 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。

```c#
public int[] NextGreaterElement(int[] nums1, int[] nums2) {
        List<int> stack = new List<int>();
        int n1 = nums1.Length;
        int n2 = nums2.Length;
        for (int i = 0; i < n1; i++)
        {
            int max = -1;
            int find = 0;
            int value = nums1[i];

            for ( int j = 0 ; j < n2; j++)
            {
                if (value == nums2[j])
                {
                    find = j;
                    break;
                };
            }
            if (find != n2)
            {
                for ( int k = find; k < n2; k++)
                {
                    if (value < nums2[k])
                    {
                        max = nums2[k];
                        break;
                    }
                }
            }
            stack.Add(max);
        }
        return stack.ToArray();
    }
```
