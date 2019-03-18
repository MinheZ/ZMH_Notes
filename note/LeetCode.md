# LeetCode Top100 stars

* [1 两数之和](#两数之和)
* [2 两数相加](#两数相加)
* [3 无重复字符的最长子串](#无重复字符的最长子串)
* [4 最长的回文子串](#最长的回文子串)
* [20 有效的括号](#有效的括号)
* [21 合并两个有序链表](#合并两个有序链表)
* [104 二叉树的最大深度](#二叉树的最大深度)
* [121 买卖股票的最佳时机](#买卖股票的最佳时机)
* [136 只出现一次的数字](#只出现一次的数字)
* [141 环形链表](#环形链表)
* [155 最小栈](#最小栈)
* [166 相交链表](#相交链表)
* [198 打家劫舍](#打家劫舍)
* [234 回文链表](#回文链表)
* [283 移动零](#移动零)
* [437 路径总和](#路径总和)
* [438 找到字符串中所有字母异位词](#找到字符串中所有字母异位词)

--------------------

## [两数之和](https://leetcode-cn.com/problems/two-sum/)

## 题目描述-Easy
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

    示例:

    给定 `nums = [2, 7, 11, 15], target = 9`

    因为 `nums[0] + nums[1] = 2 + 7 = 9`
    所以返回 `[0, 1]`

### 解题思路
用过的数字保存在map里面。
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int temp = target - nums[i];
        if (map.containsKey(temp))
            return new int[]{map.get(temp), i};
        else
            map.put(nums[i], i);
    }
    return null;
}
```
## [两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

## 题目描述-Medium
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

    示例：

    输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
    输出：7 -> 0 -> 8
    原因：342 + 465 = 807

### 解题思路
遍历链表，时间复杂度`O(N)`
```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    if (l1 == null && l2 != null)
        return l2;
    if (l1 != null && l2 == null)
        return l1;
    if (l1 == null && l2 == null)
        return null;

    int carry = 0;
    ListNode result = null;
    ListNode current = result;

    while (l1 != null || l2 != null) {
        int num1 = l1 != null ? l1.val : 0;
        int num2 = l2 != null ? l2.val : 0;
        int sum = carry + num1 + num2;

        ListNode newNode = new ListNode(sum % 10);
        carry = sum / 10;
        if (result == null)
            result = newNode;
        else
            current.next = newNode;
        current = newNode;
        if (l1 != null)  l1 = l1.next;
        if (l2 != null)  l2 = l2.next;
    }
    if (carry > 0)
        current.next = new ListNode(carry);
    return result;
}
```
## [无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

## 题目描述
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

    示例 1:

    输入: "abcabcbb"
    输出: 3
    解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
    示例 2:

    输入: "bbbbb"
    输出: 1
    解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
    示例 3:

    输入: "pwwkew"
    输出: 3
    解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
         请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
### 解题思路
```java
public int lengthOfLongestSubstring(String s) {
        if (s.length() == 0 || s == null)
            return 0;
        char[] chars = s.toCharArray();
        int length = 1;
        int left = 0;
        int right = 0;

        for (; right < chars.length; right++) {
            for (int index = left; index < right; index++) {
                if (chars[index] == chars[right]) {
                    length = length > right - left ? length : right - left;
                    left = index + 1;
                    break;
                }
            }
        }
        length = length > right - left ? length : right - left;
        return length;
    }
```

## [最长的回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

## 题目描述

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例 2：**

```
输入: "cbbd"
输出: "bb"
```

### 解题思路

## [有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

## 题目描述
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

    示例 1:

    输入: "()"
    输出: true
    示例 2:

    输入: "()[]{}"
    输出: true
    示例 3:

    输入: "(]"
    输出: false
    示例 4:

    输入: "([)]"
    输出: false
    示例 5:

    输入: "{[]}"
    输出: true

### 解题思路
```java
public boolean isValid(String s) {
    if (s.length() % 2 != 0)
        return false;
    Stack<Character> stack = new Stack<>();
    for (int i=0; i<s.length(); i++){
        char c = s.charAt(i);
        switch (c){
            case '(' :
            case '[' :
            case '{' :
                stack.push(c);
                break;
            case ')' :
            case ']' :
            case '}' :
                if (!stack.empty()){
                    char pop = stack.pop();
                    if (c == '}' && pop != '{' || c == ']' && pop != '[' ||c == ')' && pop != '(')
                        return false;
                }else
                    return false;
                break;
        }
    }
    return stack.empty() ? true : false;
}
```
## [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

## 题目描述
将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

    示例：

    输入：1->2->4, 1->3->4
    输出：1->1->2->3->4->4

### 解题思路
空间换时间
```java
 public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    // 类似归并排序中的合并过程
    ListNode dummyHead = new ListNode(0);
    ListNode cur = dummyHead;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            cur.next = l1;
            cur = cur.next;
            l1 = l1.next;
        } else {
            cur.next = l2;
            cur = cur.next;
            l2 = l2.next;
        }
    }
    // 任一为空，直接连接另一条链表
    if (l1 == null) {
        cur.next = l2;
    } else {
        cur.next = l1;
    }
    return dummyHead.next;
}
```
## [二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

## 题目描述
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

    示例：
    给定二叉树 [3,9,20,null,null,15,7]，

        3
       / \
      9  20
        /  \
       15   7
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
## [买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
## 题目描述
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

    示例 1:

    输入: [7,1,5,3,6,4]
    输出: 5
    解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
         注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
    示例 2:

    输入: [7,6,4,3,1]
    输出: 0
    解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
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
## [只出现一次的数字](https://leetcode-cn.com/problems/single-number/)
## 题目描述
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？
    示例 1:

    输入: [2,2,1]
    输出: 1
    示例 2:

    输入: [4,1,2,1,2]
    输出: 4
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

## [环形链表](https://leetcode.com/problems/linked-list-cycle/)

### 题目描述
给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

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
## [最小栈](https://leetcode.com/problems/min-stack/)
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
## [相交链表](https://leetcode.com/problems/intersection-of-two-linked-lists/)
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
## [打家劫舍](https://leetcode.com/problems/house-robber/)
### 题目描述
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

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
## [回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/submissions/)
### 题目描述
请判断一个链表是否为回文链表。

    示例 1:

    输入: 1->2
    输出: false
    示例 2:

    输入: 1->2->2->1
    输出: true
    进阶：
    你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？
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
## [移动零](https://leetcode-cn.com/problems/move-zeroes/submissions/)
### 题目描述
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

    示例:

    输入: [0,1,0,3,12]
    输出: [1,3,12,0,0]
    说明:

    必须在原数组上操作，不能拷贝额外的数组。
    尽量减少操作次数。
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
## [路径总和](https://leetcode-cn.com/problems/path-sum-iii/)
### 题目描述
给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

    示例：

    root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

          10
         /  \
        5   -3
       / \    \
      3   2   11
     / \   \
    3  -2   1

    返回 3。和等于 8 的路径有:

    1.  5 -> 3
    2.  5 -> 2 -> 1
    3.  -3 -> 11
### 解题思路
双重递归。思路：首先先序递归遍历每个节点，再以每个节点作为起始点递归寻找满足条件的路径。
```java
public int pathSum(TreeNode root, int sum) {
    if (root == null)
        return 0;
    int res = 0;
    return dfs(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
}
private int dfs (TreeNode root, int sum) {
    if (root == null)
        return 0;
    int res = 0;
    if (root.val == sum)  res++;
    res += dfs(root.left, sum - root.val);
    res += dfs(root.right, sum - root.val);
    return res;
}
```
动态规划，在一个数组中记录下每一条路径的长度，避免重复计算。
```java
int path = 0;
    public int pathSum(TreeNode root, int sum) {
        if (root == null)
            return 0;
        dfs(root, sum, new int[0]);
        return path;
    }
private void dfs (TreeNode node, int sum, int[] array) {
    if (node == null)
        return ;
    int[] newArray = new int[array.length + 1];
    for (int i=0; i<array.length; i++) {
        newArray[i] = array[i] + node.val;
        path = newArray[i] == sum ? ++path : path;
    }
    newArray[array.length] = node.val;
    path = node.val == sum ? ++path : path;
    if (node.left != null) dfs(node.left, sum, newArray);
    if (node.right != null) dfs(node.right, sum, newArray);
}
```
## [找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

### 题目描述
给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。

说明：

字母异位词指字母相同，但排列不同的字符串。
不考虑答案输出的顺序。
    示例 1:

    输入:
    s: "cbaebabacd" p: "abc"

    输出:
    [0, 6]

    解释:
    起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
    起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
     示例 2:

    输入:
    s: "abab" p: "ab"

    输出:
    [0, 1, 2]

    解释:
    起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
    起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
    起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。

### 解题思路
滑动窗口算法。
```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> list = new ArrayList<>();
    if (s == null || s.length() == 0 || p == null || p.length() == 0) return list;
    int[] hash = new int[256]; //  hash 数组
    // 保存字符的计数值
    for (char c : p.toCharArray()) {
        hash[c]++;
    }

    int left = 0, right = 0, count = p.length();
    while (right < s.length()) {
        // 每次右移都将对应的 hash 值减一
        if (hash[s.charAt(right++)]-- >= 1) count--;
        // 差异度为0，加入结果集合中
        if (count == 0) list.add(left);
        // 如果当窗口大小一定的时候即窗口大小和需要比较的字符串大小一致的时候，
        // 将窗口左边的指针向右边移动，移动的同时左边的字符计数因为在第一个if的地方hash值减小过，
        // 所以需要执行对应恢复操作，即：hash值增加，count计数值增加。
        if (right - left == p.length() && hash[s.charAt(left++)]++ >= 0) count++;
    }
    return list;
}
```

-----------------------------

<!-- ### 题目描述

### 解题思路 -->
