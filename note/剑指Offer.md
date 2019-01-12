* [栈和队列](#栈和队列)
* [查找和排序](#查找和排序)
* [递归和循环](#递归和循环)
----------------------

* [1 用两个栈实现队列](#用两个栈实现队列)
* [2 旋转数组的最小数字](#旋转数组的最小数字)
* [3 斐波那契数列](#斐波那契数列)
* [4 跳台阶](#跳台阶)
* [5 变态跳台阶](#变态跳台阶)


------------------------

# 查找和排序
## [旋转数组的最小数字](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?tpId=13&tqId=11159&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
**题目描述**

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。

NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
### 解题思路

-----------------------

# 递归和循环
## [斐波那契数列](https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3?tpId=13&tqId=11160&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
**题目描述** :fireworks:

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。 n<=39

### 解题思路
斐波那契数列（Fibonacci sequence），又称黄金分割数列、因数学家列昂纳多·斐波那契（Leonardoda Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：1、1、2、3、5、8、13、21、34、……在数学上，斐波纳契数列以如下被以递推的方法定义：
F(1)=1, F(2)=1, F(N)=F(n-1)+F(n-2)（n>=3，n∈N*）
```java
package fibonacci;

public class Fibonacci {
    public int solution(int n){
        if (n == 0)
            return 0;
        if (n == 1)
            return 1;
        if (n == 2)
            return 1;
        return solution(n-1) + solution(n-2);
    }
}
```
但是上述的解法有很严重的效率问题，通过求解第10项的调用过程图来分析：
<div align="center"><img src="../pics//1547293399(1).png" width="400px"></div>
从上图中不难发现：在这棵树中有很多结点是重复的，而且重复的结点数会随着n的增大而急剧增加，这意味计算量会随着n的增大而急剧增大。事实上，用递归方法计算的时间复杂度是以n的指数的方式递增的。

**改进解法**

改进的方法并不复杂。上述递归代码之所以慢是因为重复的计算太多，我们只要想办法避免重复计算就行了。这里的办法是从下往上计算，首先根据f(0)和f(1)算出f(2)，再根据f(1)和f(2)算出f(3)……依此类推就可以算出第n项了。很容易理解，这种思路的时间复杂度是O(n)。
```java
public int solution2(int n){
        int[] pre = {0,1};
        if (n < 2){
            return pre[n];
        }
        int f0 = 0;
        int f1 = 1;
        int fn = 0;

        for (int i=2; i<=n; i++){
            fn = f0 + f1;
            f0 = f1;
            f1 = fn;
        }
        return fn;
    }
```

## [跳台阶](https://www.nowcoder.com/practice/8c82a5b80378478f9484d87d1c5f12a4?tpId=13&tqId=11161&tPage=1&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)
**题目描述**

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

### 解题思路
列出前几种情况:

    当n=1时，fn=1;
    当n=2时，fn=2;
    当n=3时，fn=3;
    当n=4时，fn=5;
    当n=5时，fn=8;
容易看出，总的跳法是一个[斐波那契数列](#斐波那契数列)。

## [变态跳台阶](https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387?tpId=13&tqId=11162&tPage=1&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)
**题目描述**

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

### 解题思路



-----------------------

# 栈和队列
## [用两个栈实现队列](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6?tpId=13&tqId=11158&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
**题目描述**

用两个栈来实现一个队列，完成队列的Push和Pop操作。队列中的元素为int类型。

### 解题思路
队列的特征为先进先出(FIFO)，而栈的特点为先进后出。由stack1处理push操作，stack2处理pop操作。需要注意的是，每一次向stack1中添加新元素时，都需要把stack2中剩余的元素先添加入stack1。
```java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        // 重新将2里面的元素倒入1中
        while (!stack2.empty()){
            stack1.push(stack2.pop());
        }
        stack1.push(node);
    }

    public int pop() {
        while (!stack1.empty()){
            stack2.push(stack1.pop());
        }

        return stack2.pop();
    }
}
```

-----------------------------
