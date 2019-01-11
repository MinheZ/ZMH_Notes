* [栈和队列](#栈和队列)
* [查找和排序](#查找和排序)

----------------------

* [1 用两个栈实现队列](#用两个栈实现队列)
* [2 旋转数组的最小数字](#旋转数组的最小数字)



------------------------

# 查找和排序
## [旋转数组的最小数字](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?tpId=13&tqId=11159&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
**题目描述**

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。

NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
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
