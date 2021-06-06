# LeetCode Top100 stars :fountain_pen:

- [1~100](https://github.com/MinheZ/ZMH_Notes/blob/master/note/LeetCode1~100.md)
- [101~400](https://github.com/MinheZ/ZMH_Notes/blob/master/note/LeetCode101~400.md)



* [437 路径总和](#437-路径总和)
* [438 找到字符串中所有字母异位词](#438-找到字符串中所有字母异位词)
* [461 汉明距离](#461-汉明距离)
* [448 找到所有数组中消失的数字](#448-找到所有数组中消失的数字)
* [538 把二叉搜索树转换为累加树](#538-把二叉搜索树转换为累加树)
* [543 二叉树的直径](#543-二叉树的直径)
* [572 另一个树的子树](#572-另一个树的子树)
* [581. 最短无序连续子数组](#581.-最短无序连续子数组)
* [617. 合并二叉树](#617.-合并二叉树)
* [771. 宝石与石头](#771.-宝石与石头)

--------------------

## [437-路径总和](https://leetcode-cn.com/problems/path-sum-iii/)

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
    }
        path = newArray[i] == sum ? ++path : path;
    newArray[array.length] = node.val;
    path = node.val == sum ? ++path : path;
    if (node.left != null) dfs(node.left, sum, newArray);
    if (node.right != null) dfs(node.right, sum, newArray);
}
```
## [438 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

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
## [448 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)
### 题目描述
给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

    示例:
    
    输入:
    [4,3,2,7,8,2,3,1]
    
    输出:
    [5,6]
### 解题思路
将`Math.abs(nums[i]) - 1`对应位置上面数字变为负数，不存在的索引上面的数字不会发生变化。
```java
public List<Integer> findDisappearedNumbers(int[] nums) {
    List<Integer> res = new ArrayList<>();
    for (int i=0; i<nums.length; i++) {
        nums[Math.abs(nums[i]) - 1] = - Math.abs(nums[Math.abs(nums[i]) - 1]);
    }
    for (int i=0; i<nums.length; i++) {
        if (nums[i] > 0)
            res.add(i+1);
    }
    return res;
}
```
##  [461 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

### 题目描述
两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 x 和 y，计算它们之间的汉明距离。

    注意：
    0 ≤ x, y < 2^31.
    
    示例:
    
    输入: x = 1, y = 4
    
    输出: 2
    
    解释:
    1   (0 0 0 1)
    4   (0 1 0 0)
           ↑   ↑
    
    上面的箭头指出了对应二进制位不同的位置。
### 解题思路
先异或，再数 `1` 的个数。
```java
public int hammingDistance(int x, int y) {
    int cnt = 0;
    int z = x ^ y;
    while (z != 0) {
        z &= z - 1;
        cnt++;
    }
    return cnt;
}
```
## [538 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

### 题目描述
给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

    例如：
    
    输入: 二叉搜索树:
                  5
                /   \
               2     13
    
    输出: 转换为累加树:
                 18
                /   \
              20     13
### 解题思路
注意：给出的是一个二叉搜索树，数据已经有序，则按照右节点->根节点->左节点的访问顺序遍历即可。
```java
private int num = 0;
public TreeNode convertBST(TreeNode root) {
    unPreOrder(root);
    return root;
}
private void unPreOrder(TreeNode root) {
    if (root == null) {
        return;
    }
    unPreOrder(root.right);
    // 当前节点的值先加上num
    root.val += num;
    num = root.val;
    unPreOrder(root.left);
}
```
## [543 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

### 题目描述
给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

    示例 :
    给定二叉树
    
              1
             / \
            2   3
           / \
          4   5
    返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。
    
    注意：两结点之间的路径长度是以它们之间边的数目表示。

### 解题思路
整体思路就是求根节点左右子树的最大高度。
```java
private int res = 0;
public int diameterOfBinaryTree(TreeNode root) {
    deepth(root);
    return res;
}
private int deepth(TreeNode root) {
    if (root == null)
        return 0;
    int l = root.left == null ? 0 : deepth(root.left) + 1;
    int r = root.right == null ? 0 : deepth(root.right) + 1;
    res = Math.max(res, l + r);
    return Math.max(l,r);
}
```
## [572 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

### 题目描述
给定两个非空二叉树 s 和 t，检验 s 中是否包含和 t 具有相同结构和节点值的子树。s 的一个子树包括 s 的一个节点和这个节点的所有子孙。s 也可以看做它自身的一棵子树。

    示例 1:
    给定的树 s:
    
         3
        / \
       4   5
      / \
     1   2
    给定的树 t：
    
       4
      / \
     1   2
    返回 true，因为 t 与 s 的一个子树拥有相同的结构和节点值。

### 解题思路

```java
public boolean isSubtree(TreeNode s, TreeNode t) {
        return s != null && (isSameTree(s, t) || isSubtree(s.left, t) || isSubtree(s.right, t));
    }
    private boolean isSameTree(TreeNode s, TreeNode t) {
        return s == null && t == null || s != null && t != null && s.val == t.val && isSameTree(s.left, t.left) && isSameTree(s.right, t.right);
    }
```

## [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

### 题目描述

你找到的子数组应是**最短**的，请输出它的长度。
给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

**示例 1:**

```
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```

**说明 :**

1. 输入的数组长度范围在 [1, 10,000]。
2. 输入的数组可能包含**重复**元素 ，所以**升序**的意思是**<=。**

### 解题思路

```java
public int findUnsortedSubarray(int[] nums) {
        int n = nums.length, start = -1, end = -2;
        int mn = nums[n - 1], mx = nums[0];
        for (int i = 1; i < n; ++i) {
            mx = (mx > nums[i]) ? mx : nums[i];
            mn = (mn < nums[n - 1 - i]) ? mn : nums[n - 1- i];
            if (mx > nums[i]) end = i;
            if (mn < nums[n - 1 - i]) start = n - 1 - i;
        }
        return end - start + 1;
    }
```

## [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

### 题目描述

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则**不为** NULL 的节点将直接作为新二叉树的节点。

**示例 1:**

```
输入:
	Tree 1                     Tree 2
          1                         2
         / \                       / \
        3   2                     1   3
       /                           \   \
      5                             4   7
输出:
合并后的树:
	     3
	    / \
	   4   5
	  / \   \
	 5   4   7
```

**注意:** 合并必须从两个树的根节点开始。

### 解题思路

递归，中序遍历二叉树。

```java
public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 != null)
            return t2;
        if (t1 != null && t2 == null)
            return t1;
        if (t1 == null && t2 == null)
            return null;
        TreeNode root = new TreeNode(t1.val + t2.val);
        root.left = mergeTrees(t1.left, t2.left);
        root.right = mergeTrees(t1.right, t2.right);
        return root;
    }
```

## [771. 宝石与石头](https://leetcode-cn.com/problems/jewels-and-stones/)

### 题目描述

给定字符串`J` 代表石头中宝石的类型，和字符串 `S`代表你拥有的石头。 `S` 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

`J` 中的字母不重复，`J` 和 `S`中的所有字符都是字母。字母区分大小写，因此`"a"`和`"A"`是不同类型的石头。

**示例 1:**

```
输入: J = "aA", S = "aAAbbbb"
输出: 3
输入: 1->1->2
输出: 1->2
```

**示例 2:**

```
输入: J = "z", S = "ZZ"
输出: 0
```

**注意:**

- `S` 和 `J` 最多含有50个字母。
-  `J` 中的字符不重复。

### 解题思路

hash的思想。

```java
public int numJewelsInStones(String J, String S) {
        int[] arr = new int[125];
        int cnt = 0;
        for (int i=0; i<S.length(); i++) {
            arr[S.charAt(i)]++;
        }
        for (int i=0; i<J.length(); i++) {
            cnt += arr[J.charAt(i)];
        }
        return cnt;
    }
```



-----------------------------

<!-- ### 题目描述

### 解题思路 -->
输入: 1->1->2->3->3
输出: 1->2->3
```
 No newline at end of file
```