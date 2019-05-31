# LeetCode 101~400 :turtle:

- [101. 对称二叉树](#101.-对称二叉树)
- [104 二叉树的最大深度](#104-二叉树的最大深度)
- [121 买卖股票的最佳时机](#121-买卖股票的最佳时机)
- [122. 买卖股票的最佳时机 II](#122.-买卖股票的最佳时机-II)
- [136 只出现一次的数字](#136-只出现一次的数字)
- [141 环形链表](#141-环形链表)
- [155 最小栈](#155-最小栈)
- [160 相交链表](#160-相交链表)
- [198 打家劫舍](#198打家劫舍)
- [203. 移除链表元素](#203.-移除链表元素)
- [206. 反转链表](#206.-反转链表)
- [234 回文链表](#234-回文链表)
- [283 移动零](#283-移动零)

----------------------------------------------

## 101. 对称二叉树

### [题目描述](https://leetcode-cn.com/problems/symmetric-tree/)

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

### 解题思路

递归。

```java
public boolean isSymmetric(TreeNode root) {
    return isMirror(root, root);
}

private boolean isMirror(TreeNode r1, TreeNode r2) {
    if (r1 == null && r2 == null) return true;
    if (r1 == null || r2 == null) return false;
    return (r1.val == r2.val) && isMirror(r1.left, r2.right) && isMirror(r1.right, r2.left);
}
```





## [104-二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

### 题目描述

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

```
示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

### 解题思路

递归，求子树的高度。

```java
public int maxDepth(TreeNode root) {
    if (root == null)
        return 0;
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
```

## [121-买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

### 题目描述

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

```
示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

### 解题思路

如果没有更低的买入价格，则不需要比较卖出价钱。

```java
public int maxProfit(int[] prices) {
    if (prices.length <= 1)
        return 0;
    int minBuy = prices[0];
    int maxProfit = 0;
    for (int i=1; i<prices.length; i++) {
        if (minBuy > prices[i])
            minBuy = prices[i];
        else if (prices[i] - minBuy > maxProfit){
            maxProfit = prices[i] - minBuy;
        }
    }
    return maxProfit;
}
```

## 122. 买卖股票的最佳时机 II

### [题目描述](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

### 解题思路

```java
public int maxProfit(int[] prices) {
    if (prices.length == 0 || prices.length == 1)
        return 0;
    int buy = prices[0];
    int money = 0, cur = 0;
    for (int i=1; i<prices.length; i++) {
        int sale = prices[i];
        if (buy < sale) {
            if (sale - buy > cur) {   // 如果卖出利润大于之前的，则记下
                cur = sale - buy;
                if (i == prices.length - 1)
                    money += cur;
            } else {
                buy = prices[i];
                money += cur;
                cur = 0;
            }
        } else {
            buy = prices[i];
            money += cur;
            cur = 0;
        }
    }
    return money;
}
```



## [136-只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

### 题目描述

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？
    示例 1:

```
输入: [2,2,1]
输出: 1
示例 2:

输入: [4,1,2,1,2]
输出: 4
```

### 解题思路

相同的数字异或为0。

```java
public int singleNumber(int[] nums) {
    int ret = 0;
    for (int i : nums) {
        ret ^= i;
    }
    return ret;
}
```

## [141-环形链表](https://leetcode.com/problems/linked-list-cycle/)

### 题目描述

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

```
示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。

示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。

示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。

进阶：

你能用 O(1)（即，常量）内存解决此问题吗？
```

### 解题思路

2个链表指针，一个一次走2步，一个一次走一步，有环则一定会相遇。注释的部分是取巧的办法，设想的是走过的节点设一个标志位，如果起始链表中有标志位，则会判断错误。

```java
public boolean hasCycle(ListNode head) {
    if (head == null)
        return false;

//         while (head != null) {
//             if (head.val == Integer.MAX_VALUE) {
//                 return true;
//             }
//             head.val = Integer.MAX_VALUE;
//             head = head.next;
//         }
//         return false;
    ListNode fast = head;
    ListNode slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow)
            return true;
    }
    return false;
}
```

## [155-最小栈](https://leetcode.com/problems/min-stack/)

### 题目描述

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

- push(x) -- 将元素 x 推入栈中。
- pop() -- 删除栈顶的元素。
- top() -- 获取栈顶元素。
- getMin() -- 检索栈中的最小元素。

```
    示例:
    MinStack minStack = new MinStack();
    minStack.push(-2);
    minStack.push(0);
    minStack.push(-3);
    minStack.getMin();   --> 返回 -3.
    minStack.pop();
    minStack.top();      --> 返回 0.
    minStack.getMin();   --> 返回 -2.
```

### 解题思路

当有新的最小值的时候，先入栈之前最小值，再入栈最近最小值。当有最小值出栈时，要连续`stack.pop()`2次，且第二次的值为新的 `min`。

```java
Stack<Integer> stack;
int min;
/** initialize your data structure here. */
public MinStack() {
    stack = new Stack<>();
    min = Integer.MAX_VALUE;
}

public void push(int x) {
    if (x <= min) {
        stack.push(min);
        min = x;
    }
    stack.push(x);
}

public void pop() {
    if (stack.empty())
        return;
    if (stack.pop() == min)
        min = stack.pop();
}

public int top() {
    if (!stack.empty())
        return stack.peek();
    else
        throw new RuntimeException("error");
}

public int getMin() {
    if (!stack.empty())
        return min;
    else
        throw new RuntimeException("error");
}
```

## [160-相交链表](https://leetcode.com/problems/intersection-of-two-linked-lists/)

### 题目描述

编写一个程序，找到两个单链表相交的起始节点。

### 解题思路

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode l1 = headA;
    ListNode l2 = headB;
    while (l1 != l2) {
        l1 = l1 == null ? l1 = headB : l1.next;
        l2 = l2 == null ? l2 = headA : l2.next;
    }
    return l1;
}
```

## [198-打家劫舍](https://leetcode.com/problems/house-robber/)

### 题目描述

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

```
示例 1:

输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2:

输入: [2,7,9,3,1]
输出: 12
解释: 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

### 解题思路

典型的动态规划问题。

```java
public int rob(int[] nums) {
    int n = nums.length;
    if (n <= 1) return n == 0 ? 0 : nums[0];
    int[] memo = new int[n];
    memo[0] = nums[0];
    memo[1] = Math.max(nums[0], nums[1]);
    for (int i = 2; i < n; i++)
        memo[i] = Math.max(memo[i - 1], nums[i] + memo[i - 2]);
    return memo[n - 1];
}
```

## 203. 移除链表元素

### [题目描述](https://leetcode-cn.com/problems/remove-linked-list-elements/)

删除链表中等于给定值 **val** 的所有节点。

**示例:**

```
输入: 1->2->6->3->4->5->6, val = 6
输出: 1->2->3->4->5
```

### 解题思路

主要额外考虑的点在于，当头结点就是需要删除的节点时，需要多判断一下。

```java
public ListNode removeElements(ListNode head, int val) {
    if (head == null) 
        return null;

    ListNode dummy = new ListNode(-1);  // 哨兵节点
    dummy.next = head;
    ListNode cur = dummy;    // 当前节点
    ListNode last = dummy;  // 上一个节点

    while (cur != null) {
        if (cur.val == val) {             
            last.next = cur.next;
            cur = cur.next;
        }
        else {
            last = cur; // last向后移动一格
            cur = cur.next;
        }
    }
}
```

## 206. 反转链表

### [题目描述](https://leetcode-cn.com/problems/reverse-linked-list/)

反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

### 解题思路

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null)
        return head;

    ListNode last = null, cur = head, curNext = null;
    while (cur != null) {
        curNext = cur.next;
        cur.next = last;
        last = cur;
        cur = curNext;
    }
    return last;
}
```





## [283-回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/submissions/)

### 题目描述

请判断一个链表是否为回文链表。

```
示例 1:

输入: 1->2
输出: false
示例 2:

输入: 1->2->2->1
输出: true
进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？
```

### 解题思路

先找出链表的中点，然后将后半段链表反转。

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null)
        return true;

    ListNode fast = head;
    ListNode slow = head;

    while (fast.next != null && fast.next.next != null) {
        fast = fast.next.next;
        slow = slow.next;
    }
    // 反转后半段链表
    ListNode pre = null;
    ListNode cur = slow.next;

    while (cur != null) {
        ListNode temp = cur.next;
        cur.next = pre;
        pre = cur;
        cur = temp;
    }
    while (pre != null) {
        if (head.val != pre.val)
            return false;
        head = head.next;
        pre = pre.next;
    }
    return true;
}
```

## [283-移动零](https://leetcode-cn.com/problems/move-zeroes/submissions/)

### 题目描述

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

```
示例:

输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
说明:

必须在原数组上操作，不能拷贝额外的数组。
尽量减少操作次数。
```

### 解题思路

```java
public void moveZeroes(int[] nums) {
    int i = 0, j = 0;
    while (j < nums.length) {
        if (nums[j] != 0)
            nums[i++] = nums[j];
        j++;
    }
    while (i < nums.length) {
        nums[i++] = 0;
    }
}
```

