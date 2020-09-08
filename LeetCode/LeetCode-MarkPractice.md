
- [Attention](#attention)
  - [注意事项](#注意事项)
  - [需要注意的一些API](#需要注意的一些api)
- [Record](#record)
  - [Dynamic Programming](#dynamic-programming)
    - [53 Maximum Subarray](#53-maximum-subarray)
    - [152 Maximum Product Subarray](#152-maximum-product-subarray)
  - [DFS](#dfs)
    - [994 Rotting Oranges](#994-rotting-oranges)
- [Sliding Window](#sliding-window)
  - [3 Longest Substring Without Repeating Characters](#3-longest-substring-without-repeating-characters)
  - [76 Minimum Window Substring](#76-minimum-window-substring)
  - [220 Contains Duplicate III](#220-contains-duplicate-iii)
  - [209 Minimum Size Subarray Sum](#209-minimum-size-subarray-sum)
  - [239 Sliding Window Maximum](#239-sliding-window-maximum)
- [Tree](#tree)
  - [1305 All Elements in Two Binary Search Trees](#1305-all-elements-in-two-binary-search-trees)
- [Other](#other)
  - [9 Palindrome Number](#9-palindrome-number)
  - [1 Two Sum](#1-two-sum)
  - [Maximum Subarray](#maximum-subarray)
  - [14 Longest Common Prefix](#14-longest-common-prefix)
  - [204 Count Primes](#204-count-primes)
  - [20 Valid Parentheses](#20-valid-parentheses)
  - [169 Majority Element](#169-majority-element)
  - [28 Implement strStr()](#28-implement-strstr)
  - [21 Merge Two Sorted Lists](#21-merge-two-sorted-lists)
  - [27 Remove Element](#27-remove-element)
  - [58 Length of Last Word](#58-length-of-last-word)
  - [83 Remove Duplicates from Sorted List](#83-remove-duplicates-from-sorted-list)
  - [88 Merge Sorted Array](#88-merge-sorted-array)
  - [100 Same Tree](#100-same-tree)
  - [101 Symmetric Tree](#101-symmetric-tree)
  - [436 Find Right Interval](#436-find-right-interval)
  - [442 Find All Duplicates in an Array](#442-find-all-duplicates-in-an-array)
  - [48 Rotate Image](#48-rotate-image)
  - [7 Reverse Integer](#7-reverse-integer)
  - [Valid Anagram](#valid-anagram)
  - [459 Repeated Substring Pattern](#459-repeated-substring-pattern)
  - [763 Partition Labels](#763-partition-labels)
# Attention
## 注意事项
- [刷题需要注意的小细节](LeetCode-Attention.md)
- 滑动窗口典型问题：第 3 题、第 76 题、第 438 题
- 双指针典型问题：第 11 题、第 15 题、第 42 题
## 需要注意的一些API

- **`java.lang.Character.isLetterOrDigit(int codePoint)`** 确定指定字符(Unicode代码点)是一个字母或数字。
字符被确定是字母或数字，如果不是isLetter(codePoint) 也不是 isDigit(codePoint) 的字符，则返回true。
- **`getOrDefault(Object key, V defaultValue)`** Returns the value to which the specified key is mapped, or defaultValue if this map contains no mapping for the key.
- **`Character.toLowerCase`**




# Record

| Date  | Problem  |
|:---:|:---:|
|  2020.08.29 | [53 Maximum Subarray](#53-maximum-subarray) / [152 Maximum Product Subarray](#152-maximum-product-subarray) / [442 Find All Duplicates in an Array](#442-find-all-duplicates-in-an-array) |
|2020.08.30|[994 Rotting Oranges](#994-rotting-oranges)|
|2020.09.02|[3 Longest Substring Without Repeating Characters](#3-longest-substring-without-repeating-characters) / [Minimum Window Substring](#minimum-window-substring) / [220 Contains Duplicate III](#220-contains-duplicate-iii)|





## Dynamic Programming

    一部分详见 Data Structures & Algorithms 中的专题部分

### 53 Maximum Subarray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**

    Input: [-2,1,-3,4,-1,2,1,-5,4],
    Output: 6
    Explanation: [4,-1,2,1] has the largest sum = 6.

这个题用动态规划的思路来做：
首先确定状态转移方程 `dp[i] = Math.max(dp[i - 1] + nums[i], nums[i])`


```java
class Solution {
    public int maxSubArray(int[] nums) {
        int N = nums.length;
        
        if(N == 0) return 0;
        int[] dp = new int[N];
        dp[0] = nums[0];
        int max = dp[0];
        for(int i = 1; i < N; i++){
            dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
            max = Math.max(max, dp[i]);
        }
        return max;
    }
}
```
### 152 Maximum Product Subarray

Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

**Example 1:**

    Input: [2,3,-2,4]
    Output: 6
    Explanation: [2,3] has the largest product 6.

**Example 2:**

    Input: [-2,0,-1]
    Output: 0
    Explanation: The result cannot be 2, because [-2,-1] is not a subarray.


这个题就有很多值得讲的地方了，首先看这个题和上一题 Maximum Subarray 的差异：

- 求乘积的最大值，示例中负数的出现，告诉我们这题和 53 题不一样了，一个正数乘以负数就变成负数，即：**最大值乘以负数就变成了最小值**；
- 因此：**最大值和最小值是相互转换的，这一点提示我们可以把这种转换关系设计到「状态转移方程」里去**；
- 如何解决这个问题呢？这里常见的技巧是在「状态设计」的时候，在原始的状态设计后面多加一个维度，减少分类讨论，降低解决问题的难度。

这里是百度百科的「**无后效性**」词条的解释：

    无后效性是指如果在某个阶段上过程的状态已知，则从此阶段以后过程的发展变化仅与此阶段的状态有关，而与过程在此阶段以前的阶段所经历过的状态无关。利用动态规划方法求解多阶段决策过程问题，过程的状态必须具备无后效性。

再翻译一下就是：「动态规划」通常不关心过程，只关心「阶段结果」，这个「阶段结果」就是我们设计的「状态」。什么算法关心过程呢？「回溯算法」，「回溯算法」需要记录过程，复杂度通常较高。

而将状态定义得更具体，通常来说对于一个问题的解决是满足「无后效性」的。这一点的叙述很理论化，不熟悉朋友可以通过多做相关的问题来理解「无后效性」这个概念。

**第 1 步：状态设计（特别重要）**

- `dp[i][j]`：以 `nums[i]` 结尾的连续子数组的最值，计算最大值还是最小值由 `j` 来表示，`j` 就两个值；
  - 当 `j = 0` 的时候，表示计算的是最小值；
  - 当 `j = 1` 的时候，表示计算的是最大值。

这样一来，状态转移方程就容易写出。

**第 2 步：推导状态转移方程（特别重要）**

- 由于状态的设计 `nums[i]` 必须被选取（请大家体会这一点，这一点恰恰好也是使得子数组、子序列问题更加简单的原因：当情况复杂、分类讨论比较多的时候，需要固定一些量，以简化计算）；

- `nums[i]` 的正负和之前的状态值（正负）就产生了联系，由此关系写出状态转移方程：

  - 当 `nums[i] > 0` 时，由于是乘积关系：
    - 最大值乘以正数依然是最大值；
    - 最小值乘以同一个正数依然是最小值；
  - 当 `nums[i] < 0` 时，依然是由于乘积关系：
    - 最大值乘以负数变成了最小值；
    - 最小值乘以同一个负数变成最大值；
  - 当 `nums[i] = 0` 的时候，由于 `nums[i]` 必须被选取，最大值和最小值都变成 00 ，合并到上面任意一种情况均成立。
- 但是，还要注意一点，之前状态值的正负也要考虑：例如，在考虑最大值的时候，当 `nums[i] > 0` 是，如果 `dp[i - 1][1] < 0`（之前的状态最大值） ，此时 `nums[i]` 可以另起炉灶（这里依然是第 53 题的思想），此时 `dp[i][1] = nums[i]` ，合起来写就是：

`dp[i][1] = max(nums[i], nums[i] * dp[i - 1][1]) if nums[i] >= 0`

其它三种情况可以类似写出，状态转移方程如下：

```
dp[i][0] = min(nums[i], nums[i] * dp[i - 1][0]) if nums[i] >= 0
dp[i][1] = max(nums[i], nums[i] * dp[i - 1][1]) if nums[i] >= 0

dp[i][0] = min(nums[i], nums[i] * dp[i - 1][1]) if nums[i] < 0
dp[i][1] = max(nums[i], nums[i] * dp[i - 1][0]) if nums[i] < 0
```
**第 3 步：考虑初始化**

由于 `nums[i]` 必须被选取，那么 `dp[i][0] = nums[0]，dp[i][1] = nums[0]`。

```java
class Solution {
    public int maxProduct(int[] nums) {
        int N = nums.length;
        int[][] dp = new int[N][2];
        // dp[N][0] 表示最小值
        // dp[N][1] 表示最大值
        dp[0][0] = nums[0];
        dp[0][1] = nums[0];
        int res = nums[0];
        for(int i = 1; i < N; i++){
            if(nums[i] >= 0){
                dp[i][0] = Math.min(nums[i], dp[i - 1][0] * nums[i]);
                dp[i][1] = Math.max(nums[i], dp[i - 1][1] * nums[i]);
            }else{
                dp[i][0] = Math.min(nums[i], dp[i - 1][1] * nums[i]);
                dp[i][1] = Math.max(nums[i], dp[i - 1][0] * nums[i]);
            }
            res = Math.max(res, dp[i][1]);
        }
        return res;
    }
}
```
## DFS

**什么情况应当用 BFS 搜索**

我们都知道 DFS（深度优先搜索）和 BFS（广度优先搜索）。它们各有不同的适应场景。

![](https://pic.leetcode-cn.com/725e473003c35e3be67ac6177cc6744fa04b0466795b5e69c7d673f626206b86-file_1583293748397)

BFS 可以看成是层序遍历。从某个结点出发，BFS 首先遍历到距离为 1 的结点，然后是距离为 2、3、4…… 的结点。因此，BFS 可以用来**求最短路径问题**。BFS 先搜索到的结点，一定是距离最近的结点。

再看看这道题的题目要求：返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。翻译一下，实际上就是求**腐烂橘子到所有新鲜橘子的最短路径**。那么这道题使用 BFS，应该是毫无疑问的了。

**如何写（最短路径的） BFS 代码**

我们都知道 BFS 需要使用队列，代码框架是这样子的（伪代码）：

```python
while queue 非空:
	node = queue.pop()
    for node 的所有相邻结点 m:
        if m 未访问过:
            queue.push(m)
```
但是用 BFS 来求最短路径的话，这个队列中第 1 层和第 2 层的结点会紧挨在一起，无法区分。因此，我们需要稍微修改一下代码，在每一层遍历开始前，记录队列中的结点数量 nn ，然后一口气处理完这一层的 nn 个结点。代码框架是这样的：

```python
depth = 0 # 记录遍历到第几层
while queue 非空:
    depth++
    n = queue 中的元素个数
    循环 n 次:
        node = queue.pop()
        for node 的所有相邻结点 m:
            if m 未访问过:
                queue.push(m)
```


### 994 Rotting Oranges
In a given grid, each cell can have one of three values:

- the value 0 representing an empty cell;
- the value 1 representing a fresh orange;
- the value 2 representing a rotten orange.

Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.

**Example 1:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200830000451.png)

    Input: [[2,1,1],[1,1,0],[0,1,1]]
    Output: 4

**Example 2:**

    Input: [[2,1,1],[0,1,1],[1,0,1]]
    Output: -1
    Explanation:  The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**

    Input: [[0,2]]
    Output: 0
    Explanation:  Since there are already no fresh oranges at minute 0, the answer is just 0.

有了计算最短路径的层序 BFS 代码框架，写这道题就很简单了。这道题的主要思路是：

一开始，我们找出所有腐烂的橘子，将它们放入队列，作为第 0 层的结点。
然后进行 BFS 遍历，每个结点的相邻结点可能是上、下、左、右四个方向的结点，注意判断结点位于网格边界的特殊情况。
由于可能存在无法被污染的橘子，我们需要记录新鲜橘子的数量。在 BFS 中，每遍历到一个橘子（污染了一个橘子），就将新鲜橘子的数量减一。如果 BFS 结束后这个数量仍未减为零，说明存在无法被污染的橘子。

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int N = grid.length;
        int M = grid[0].length;
        
        Queue<int[]> queue = new LinkedList<>();
        
        int count = 0;
        
        for(int i = 0; i < N; i++){
            for(int j = 0; j < M; j++){
                if(grid[i][j] == 1){
                    count++;
                }else if(grid[i][j] == 2){
                    queue.add(new int[]{i, j});
                }
            }
        }
        
        int level = 0;
        while(count > 0 && !queue.isEmpty()){
            level++;
            int size = queue.size();
            for(int a = 0; a < size; a++){
                int[] rott = queue.poll();
                int i = rott[0];
                int j = rott[1];
                
                if(i - 1 >= 0 && grid[i - 1][j] == 1){
                    grid[i - 1][j] = 2;
                    count--;
                    queue.add(new int[]{i - 1, j});
                }
                
                if(i + 1 < N && grid[i + 1][j] ==1){
                    grid[i + 1][j] = 2;
                    count--;
                    queue.add(new int[]{i + 1, j});
                }
                              
                if(j - 1 >= 0 && grid[i][j - 1] ==1){
                    grid[i][j - 1] = 2;
                    count--;
                    queue.add(new int[]{i, j - 1});
                }
                if(j + 1 < M && grid[i][j + 1] == 1){
                    grid[i][j + 1] = 2;
                    count--;
                    queue.add(new int[]{i, j + 1});
                }
            }

        }
        if(count > 0){
            return -1;
        }else{
            return level;
        }
    }
}
```





# Sliding Window
## 3 Longest Substring Without Repeating Characters
Given a string, find the length of the longest substring without repeating characters.

**Example 1:**

    Input: "abcabcbb"
    Output: 3 
    Explanation: The answer is "abc", with the length of 3. 

**Example 2:**

    Input: "bbbbb"
    Output: 1
    Explanation: The answer is "b", with the length of 1.

**Example 3:**

    Input: "pwwkew"
    Output: 3
    Explanation: The answer is "wke", with the length of 3. 
                Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        //Sliding Window
        int N = s.length();
        int res = 0;
        Map<Character, Integer> map = new HashMap<>();
        for(int left = 0, right = 0; right < N; right++){
            if(map.containsKey(s.charAt(right))){
                left = Math.max(left, map.get(s.charAt(right)) + 1);
            }
            map.put(s.charAt(right), right);
            res = Math.max(res, right - left + 1);        
        }
        return res;
    }
}
```

## 76 Minimum Window Substring
Given a string S and a string T, find   the minimum window in S which will contain all the characters in T in complexity O(n).

**Example:**

    Input: S = "ADOBECODEBANC", T = "ABC"
    Output: "BANC"

```java
class Solution {
    public String minWindow(String s, String t) {
        if (s == null || s == "" || t == null || t == "" || s.length() < t.length()) {
            return "";
        }
        //维护两个数组，记录已有字符串指定字符的出现次数，和目标字符串指定字符的出现次数
        //ASCII表总长128
        int[] need = new int[128];
        int[] have = new int[128];

        //将目标字符串指定字符的出现次数记录
        for (int i = 0; i < t.length(); i++) {
            need[t.charAt(i)]++;
        }

        //分别为左指针，右指针，最小长度(初始值为一定不可达到的长度)
        //已有字符串中目标字符串指定字符的出现总频次以及最小覆盖子串在原字符串中的起始位置
        int left = 0, right = 0, min = s.length() + 1, count = 0, start = 0;
        while (right < s.length()) {
            char r = s.charAt(right);
            //说明该字符不被目标字符串需要，此时有两种情况
            // 1.循环刚开始，那么直接移动右指针即可，不需要做多余判断
            // 2.循环已经开始一段时间，此处又有两种情况
            //  2.1 上一次条件不满足，已有字符串指定字符出现次数不满足目标字符串指定字符出现次数，那么此时
            //      如果该字符还不被目标字符串需要，就不需要进行多余判断，右指针移动即可
            //  2.2 左指针已经移动完毕，那么此时就相当于循环刚开始，同理直接移动右指针
            if (need[r] == 0) {
                right++;
                continue;
            }
            //当且仅当已有字符串目标字符出现的次数小于目标字符串字符的出现次数时，count才会+1
            //是为了后续能直接判断已有字符串是否已经包含了目标字符串的所有字符，不需要挨个比对字符出现的次数
            if (have[r] < need[r]) {
                count++;
            }
            //已有字符串中目标字符出现的次数+1
            have[r]++;
            //移动右指针
            right++;
            //当且仅当已有字符串已经包含了所有目标字符串的字符，且出现频次一定大于或等于指定频次
            while (count == t.length()) {
                //挡窗口的长度比已有的最短值小时，更改最小值，并记录起始位置
                if (right - left < min) {
                    min = right - left;
                    start = left;
                }
                char l = s.charAt(left);
                //如果左边即将要去掉的字符不被目标字符串需要，那么不需要多余判断，直接可以移动左指针
                if (need[l] == 0) {
                    left++;
                    continue;
                }
                //如果左边即将要去掉的字符被目标字符串需要，且出现的频次正好等于指定频次，那么如果去掉了这个字符，
                //就不满足覆盖子串的条件，此时要破坏循环条件跳出循环，即控制目标字符串指定字符的出现总频次(count）-1
                if (have[l] == need[l]) {
                    count--;
                }
                //已有字符串中目标字符出现的次数-1
                have[l]--;
                //移动左指针
                left++;
            }
        }
        //如果最小长度还为初始值，说明没有符合条件的子串
        if (min == s.length() + 1) {
            return "";
        }
        //返回的为以记录的起始位置为起点，记录的最短长度为距离的指定字符串中截取的子串
        return s.substring(start, start + min);
    }
}
```
## 220 Contains Duplicate III
Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.

**Example 1:**

    Input: nums = [1,2,3,1], k = 3, t = 0
    Output: true

**Example 2:**

    Input: nums = [1,0,1,1], k = 1, t = 2
    Output: true

**Example 3:**

    Input: nums = [1,5,9,1,5,9], k = 2, t = 3
    Output: false

- [LeetCode讲解](https://leetcode-cn.com/problems/contains-duplicate-iii/solution/hua-dong-chuang-kou-er-fen-sou-suo-shu-zhao-shang-/)

暴力解法
```java
public class Solution {

    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        int len = nums.length;
        long a;
        long b;

        for (int i = 0; i < len; i++) {
            for (int j = i + 1; j < len && j <= i + k; j++) {
                a = nums[i];
                b = nums[j];
                if (Math.abs(a - b) <= t) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
平衡二叉树（滑动窗口）
```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        TreeSet<Integer> set = new TreeSet<>();
        for(int i = 0; i < nums.length; i++){
            Integer s = set.ceiling(nums[i]);
            Integer g = set.floor(nums[i]);
            if(s != null && s <= nums[i] + t || g != null && g >= nums[i] - t){
                return true;
            }
            set.add(nums[i]);
            if (set.size() > k) {
                set.remove(nums[i - k]);
            }
        }
        return false;
     }
}
```
## 209 Minimum Size Subarray Sum
Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

**Example:** 

    Input: s = 7, nums = [2,3,1,2,4,3]
    Output: 2
    Explanation: the subarray [4,3] has the minimal length under the problem constraint.

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int N = nums.length;
        int left = 0, right = 0, count = 0, minLen = N + 1;
        
        if(N == 0) return 0;
        
        while(right < N){
            count += nums[right];
            right++;
            
            while(count >= s && left <= right){
                if(right - left < minLen){
                    minLen = right - left;
                }
                count -= nums[left];
                left++;
            }
        }
        if(minLen == N + 1) return 0;
        else return minLen;
    }
} 
```
## 239 Sliding Window Maximum

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

Follow up:
Could you solve it in linear time?

**Example:**

    Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
    Output: [3,3,5,5,6,7] 
    Explanation: 

        Window position                Max
        ---------------               -----
       [1  3  -1] -3  5  3  6  7       3
        1 [3  -1  -3] 5  3  6  7       3
        1  3 [-1  -3  5] 3  6  7       5
        1  3  -1 [-3  5  3] 6  7       5
        1  3  -1  -3 [5  3  6] 7       6
        1  3  -1  -3  5 [3  6  7]      7

思路:[使用Deque双端队列](https://leetcode-cn.com/problems/sliding-window-maximum/solution/zui-da-suo-yin-dui-shuang-duan-dui-lie-cun-suo-yin/)

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int N = nums.length;
        if(N == 0) return new int[]{};
        
        int[] res = new int[N - k + 1];
         // 滑动窗口，注意：保存的是索引值
        ArrayDeque<Integer> deque = new ArrayDeque<>(k);
        
        for(int i = 0; i < N; i++){
            // 当元素从左边界滑出的时候，如果它恰恰好是滑动窗口的最大值
            // 那么将它弹出
            while(!deque.isEmpty() && deque.peekFirst == i - k){
                deque.pollFirst();
            }
            // 如果滑动窗口非空，新进来的数比队列里已经存在的数还要大
            // 则说明已经存在数一定不会是滑动窗口的最大值（它们毫无出头之日）
            // 将它们弹出
            while(!deque.isEmpty() && nums[i] >= nums[deque.peekLast()]){
                deque.pollLast();
            }
            deque.add(i);
            // 队首一定是滑动窗口的最大值的索引
            if(i >= k - 1){
                res[i - k + 1] = nums[deque.peekFirst];
            }
        }
        return res;
    }
}
```

# Tree
## 1305 All Elements in Two Binary Search Trees
Given two binary search trees root1 and root2.

Return a list containing all the integers from both trees sorted in ascending order.

 

**Example 1:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200905194207.png)

    Input: root1 = [2,1,4], root2 = [1,0,3]
    Output: [0,1,1,2,3,4]

**Example 2:**

    Input: root1 = [0,-10,10], root2 = [5,1,7,0,2]
    Output: [-10,0,0,1,2,5,7,10]

**Example 3:**

    Input: root1 = [], root2 = [5,1,7,0,2]
    Output: [0,1,2,5,7]

**Example 4:**

    Input: root1 = [0,-10,10], root2 = []
    Output: [-10,0,10]

**Example 5:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200905194218.png)


    Input: root1 = [1,null,8], root2 = [8,1]
    Output: [1,1,8,8]

普通方法,就是把两个BST中的元素遍历到一个List中,最后利用`Collections.sort()`对List进行排序操作,但是这个方法并没有利用BST的优势
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        List<Integer> list = new ArrayList<>();
        find(root1, list);
        find(root2, list);
        Collections.sort(list);
        return list;
    }
    private void find(TreeNode root, List<Integer> list){
        if(root == null) return;
        list.add(root.val);
        find(root.left, list);
        find(root.right, list);
    }
}
```
方法二,通过中序遍历,利用BST的特性,这样保存到每一个List中的元素都是有序的,最后我们再利用Merge Sort即可节省大量的时间 
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        List<Integer> list1 = new ArrayList<>();
        List<Integer> list2 = new ArrayList<>();
        dfs(root1, list1);
        dfs(root2, list2);
        return merge(list1, list2);
        
    }
    
    private void dfs(TreeNode root, List<Integer> list){
        if(root == null) return;
        dfs(root.left, list);
        list.add(root.val);
        dfs(root.right, list);
    }
    
    private List<Integer> merge(List<Integer> list1, List<Integer> list2){
        List<Integer> res = new ArrayList<>();
        int i = 0, j = 0;
        while(i < list1.size() && j < list2.size()){
            if(list1.get(i) < list2.get(j)){
                res.add(list1.get(i));
                i++;
            }else{
                res.add(list2.get(j));
                j++;
            }
        }
        while(i < list1.size()){
            res.add(list1.get(i));
            i++;
        }
        while(j < list2.size()){
            res.add(list2.get(j));
            j++;
        }
        return res;
    }
}
```







# Other
## 9 Palindrome Number

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.  
```java
class Solution {
    public boolean isPalindrome(int x) {
        String str = String.valueOf(x);
        String reverse = new StringBuilder(str).reverse().toString();
        if(reverse.equals(str)){
            return true;
        }else{
            return false;
        }
    }
}
```
## 1 Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

**Example:**  

    Given nums = [2, 7, 11, 15], target = 9,
    Because nums[0] + nums[1] = 2 + 7 = 9,
    return [0, 1]. 

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {   
        Map<Integer, Integer> map = new HashMap<>(); 
        for (int i = 0; i < nums.length; i++) {   
            if (map.containsKey(target - nums[i])) {
                return new int[]{map.get(target - nums[i]), i};
            } else {
                map.put(nums[i], i);
                }
            }
        return null;
    }
}
```
## Maximum Subarray
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.  

**Example:  **

    Input: [-2,1,-3,4,-1,2,1,-5,4],  
    Output: 6  
    Explanation: [4,-1,2,1] has the largest sum = 6.

这个题的思路是：从头开始计算，一旦出现子序列和小于0的情况，就重新开始(sum=0)，因为前面的和为负数的话，后面与它相加会变小。
```java
class Solution {
    public int maxSubArray(int[] nums) {

        int cur = nums[0];
        int sum = nums[0];

        for(int n : nums)
        {
            cur = Math.max(n, cur + n);
            sum = Math.max(sum, cur);
        }
        return sum;
    }
}
```
## 14 Longest Common Prefix
Write a function to find the longest common prefix string amongst an array of strings.  
If there is no common prefix, return an empty string "".

**Example 1:**

    Input: ["flower","flow","flight"]  
    Output: "fl"  

**Example 2:**

    Input: ["dog","racecar","car"]  
    Output: ""  

Explanation: There is no common prefix among the input strings.

思路一：取第一位作为标准，嵌套两个循环，第一个以第一个单词的长度为标准，i++，第二个循环是以整个数组的长度为标准，目的是为了遍历每一个单词。从标准单词的第一个字母开始，每一个单词都比对一下，如果有不一样的，跳出循环，如果一样，就执行下一个。
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length == 0 || strs == null){
            return "";
        }else{
            for(int i = 0; i < strs[0].length(); i++){
                for(int j = 1; j <strs.length; j++){
                    if(i >= strs[j].length() || strs[j].charAt(i) != strs[0].charAt(i)){
                        return strs[0].substring(0, i);
                    }
                }
            }
        }
        return strs[0];
    }
}
```
思路二： 这个思路比较巧妙，取第一个单词作为基准，依次和后面的单词进行比对，如果后面单词不包含第一个单词的所有字母，那就让第一个单词的最后一位减一，然后继续进行比较，知道所有单词都是以第一个单词开始为止，这个方法只需要用到一个循环。
```java
class Solution{
    public String longestCommonPrefix(String[] strs){
        if(strs.length == 0 || strs == null){
            return "";
        }
        String result = strs[0];
        for(int i = 0; i < strs.length; i++){
            if(!strs[i].starsWith(result)){
                result = result.substring(0, result.length()-1);
                i--;
            }
        }
        return result;
    }
}
```
## 204 Count Primes
Count the number of prime numbers less than a non-negative number, n.  

**Example:**  

    Input: 10  
    Output: 4  

Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

思路一:最容易想到的方法就是写两个for，让该数字从1开始除，如果可以除，那么就是素数。这个方法当面临一个特别大的数的时候，就会很耗费时间。所以不推荐
  ```java
  ```
思路二：利用数学方法：**埃拉托斯特尼筛法**  

![](https://images2017.cnblogs.com/blog/1157228/201709/1157228-20170907193558741-1720107409.gif)

首先，将2到n范围内的所有整数写下来。其中最小的数字2是素数。将表中所有2的倍数都划去。表中剩余的最小数字是3，它不能被更小的数整除，所以是素数。再将表中所有3的倍数全都划去。依次类推，如果表中剩余的最小数字是m时，m就是素数。然后将表中所有m的倍数全部划去。像这样反复操作，就能依次枚举n以内的素数。

## 20 Valid Parentheses
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:
- Open brackets must be closed by the same type of brackets.
- Open brackets must be closed in the correct order.
- Note that an empty string is also considered valid.

思路：
这个题用Stack是最简单的，利用了Stack的特性：**先进后出，后进先出**，以左半括号为，遇到左括号就把对应的右括号push进stack里面，遇到右括号就把栈里面的pop出来。
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        for(char c: s.toCharArray()){
            if(c == '('){
                stack.push(')');
            }else if(c == '{'){
                stack.push('}');
            }else if(c =='['){
                stack.push(']');
            }else if(stack.isEmpty() || stack.pop() != c){
                return false;
            }    
        }
        return stack.isEmpty();
    }        
}
```
## 169 Majority Element
Given an array of size n, find the majority element. The majority element is the element that appears **more than** ⌊ n/2 ⌋ times. You may assume that the array is non-empty and the majority element always exist in the array.

**Example 1:**

    Input: [3,2,3]
    Output: 3

**Example 2:**

    Input: [2,2,1,1,1,2,2]
    Output: 2

**思路一：**

看到这个题的时候，第一时间反应是利用hashmap存储，遍历sums中的元素，存在hashmap中，因为是返回众数，所以在存进map的时候可以直接做个判断，如果key的数量 > nums.length / 2，直接返回就行。
- 缺点就是太慢了
```java
class Solution {
    public int majorityElement(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        int res = -1;
        if(nums.length == 1){
            return nums[0];
        }
        for(int n : nums){
            if(map.containsKey(n)){
                map.put(n,map.get(n)+1);
                if(map.get(n) > nums.length / 2){
                    res = n;
                }
            }else{
                map.put(n, 1);
            }
        }
        return res;
    }
}
```
**思路二：**

思路很巧妙，直接对nums进行排序，直接返回中间的元素即可。如果是数量最多的数字(> n / 2 )，那么中间的数字必定是我们需要的结果。
```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
}
```
## 28 Implement strStr()
Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

**Example 1:**

    Input: haystack = "hello", needle = "ll"
    Output: 2

**Example 2:**

    Input: haystack = "aaaaa", needle = "bba"
    Output: -1

**Clarification:**

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().

## 21 Merge Two Sorted Lists
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

    Input: 1->2->4, 1->3->4
    Output: 1->1->2->3->4->4

**思路一：** 链表合并
 
**1. 具体思路：**

设置一个dummyhead，设置pre指针指向head，然后两个指针p,q分别指向l1,l2，当p,q不为空时，判断p.val, q.val的大小，如果p.val小,则pre.next 指向p，然后p指向下一节点。反之亦然。

当p,q中一条链表遍历完毕，则pre.next指向剩余的链表即可

最终返回dummyhead.next
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {

        ListNode dummyhead = new ListNode(0) , p = l1, q = l2, pre = dummyhead;

        while(p != null && q != null){
            if(p.val <= q.val){
                pre.next = p;
                p = p.next;
            }else{
                pre.next = q;
                q = q.next;
            }
            pre = pre.next;
        }

        if(p != null) pre.next = p;
        if(q != null) pre.next = q;

        return dummyhead.next;
    }
}
```
**2. 复杂度：**

时间复杂度| 空间复杂度 | 耗时 | 内存 
--- | --- | --- | --- 
O(n) | O(1) | 6ms | 41.3MB

**思路二：** 递归

**1. 具体思路：**

这个看别人提交的，代码的思路能理解，很难直观想到。 每次递归解决当前节点应该返回哪个节点，同时将下一节点的指向交由下一层的递归处理。
```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2){
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        if(l1.val < l2.val){
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else{
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
}
```
**2. 复杂度**

时间复杂度| 空间复杂度 | 耗时 | 内存 
--- | --- | --- | --- 
O(n) | O(1) | 6ms | 41.3MB

## 27 Remove Element
Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array** in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

    Given nums = [3,2,2,3], val = 3,

    Your function should return length = 2, with the first two elements of nums being 2.

    It doesn't matter what you leave beyond the returned length.
**Example 2:**

    Given nums = [0,1,2,2,3,0,4,2], val = 2,

    Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

    Note that the order of those five elements can be arbitrary.

    It doesn't matter what values are set beyond the returned length.

**方法一：**

**思路：**
- 原地改**数组**
- 定义k = 0；用来记录数组中不等于val的元素的个数
- 从头开始遍历数组（n =0），若数组中元素与val不相等（ 即 nums[n] != val）则将nums[n]赋给nums[k],然后将k + 1;然后执行下一次循环；若数组中元素与val值相等即（nums[n] != val），则执行下一次循环; 直到循环执行结束
- 最后返回k

**注意：**
这个题其实一开始我很晕，虽然很简单，但是要搞清楚题目的意思，不用管数组的长度和剩下的元素，只用替换掉前k个元素并且返回k就可以了！

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int k = 0;
        for(int n = 0; n < nums.length; n++){
            if(nums[n] != val){
                nums[k] = nums[n];
                k++;
            }
        }
        return k;
    }
}
```
## 58 Length of Last Word

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word (last word means the last appearing word if we loop from left to right) in the string.

If the last word does not exist, return 0.

Note: A word is defined as a maximal substring consisting of non-space characters only.

**Example:**

    Input: "Hello World"
    Output: 5

思路：
这个题没难度，为什么我要写出来呢？就是需要注意，有一个小坑，如果字符串最后一个字符是空格的话，那么常规思路写的算法，看答案学到了一个java的方法：`.trim()`，目的是去除字符串两端的多余的空格。

```java
class Solution {
    public int lengthOfLastWord(String s) {
        s = s.trim();
        int count = 0;
        for(int k = s.length() - 1; k >=0; k--){
           if(s.charAt(k) != ' '){
               count++;
           }else{
               break;
           }
        }
        return count;
    }
}
```
## 83 Remove Duplicates from Sorted List
Given a sorted linked list, delete all duplicates such that each element appear only once.

**Example 1:**

    Input: 1->1->2
    Output: 1->2    
**Example 2:**

    Input: 1->1->2->3->3
    Output: 1->2->3

思路：
很简单的一道题，考的就是对Java链表结点操作的熟练程度。在一开始我们定义一个指针`cur`，利用cur判断当前与下一个结点的val，相等的话就让cur的next指向next.next。记住每次操作完将cur向后移动一个位置就可以了。
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while(cur != null){
            while(cur.next != null && cur.val == cur.next.val){
                cur.next = cur.next.next;
            }
            cur = cur.next;
        }
        return head;
    }
}
```

## 88 Merge Sorted Array

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

**Note:**

- The number of elements initialized in nums1 and nums2 are m and n respectively.
- You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2.

**Example:**

    Input:
    nums1 = [1,2,3,0,0,0], m = 3
    nums2 = [2,5,6],       n = 3

    Output: [1,2,2,3,5,6]\

思路：
这个题其实很简单，我们只需要创建一个新数组，然后把要合并的两个数组挨个遍历，然后挨个放进新数组就可以了。但是这种思路**太简单**，我们可以不生成新数组的情况下合并，从nums1的末尾从后往前放就可以了。
还有一种方法更加暴力，直接把两个数组拼接起来，然后Array.sort()排序，这里不再写方法。

```java
 class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int index = m + n - 1;
        
        while(i >= 0 && j >= 0){
            if(nums1[i] > nums2[j]){
                nums1[index] = nums1[i];
                index--;
                i--;
            }else{
                nums1[index] = nums2[j];
                index--;
                j--;
            }
        }
        while(i >= 0){
            nums1[index] = nums1[i];
            index--;
            i--;
        }
        while(j >= 0){
            nums1[index] = nums2[j];
            index--;
            j--;
        }
    }
}
```
## 100 Same Tree
Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

**Example 1:**

    Input:    1         1
             / \       / \
            2   3     2   3

           [1,2,3],   [1,2,3]

Output: true
**Example 2:**

    Input:    1         1
             /           \
            2             2

          [1,2],     [1,null,2]

Output: false
**Example 3:**

    Input:   1         1
            / \       / \
           2   1     1   2

          [1,2,1],   [1,1,2]

Output: false

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p == null && q == null) 
            return true;
        if(p == null || q == null) 
            return false;
        if(p.val != q.val) 
            return false;
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
} 
```

## 101 Symmetric Tree
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

But the following `[1,2,2,null,3,null,3]` is not:

        1
       / \
      2   2
       \   \
       3    3
思路：利用递归
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isMirror(root, root);
    }
    
    public boolean isMirror(TreeNode t1, TreeNode t2){
        if(t1 == null && t2 == null){
            return true;
        }
        if(t1 == null || t2 == null){
            return false;    
        }
        if(t1.val != t2.val){
            return false;
        }
        return (t1.val == t2.val) && isMirror(t1.left, t2.right) && isMirror(t1.right, t2.left);
    }
}
```
## 436 Find Right Interval

- 排序+查找 / TreeMap

Given a set of intervals, for each of the interval i, check if there exists an interval j whose start point is bigger than or equal to the end point of the interval i, which can be called that j is on the "right" of i.

For any interval i, you need to store the minimum interval j's index, which means that the interval j has the minimum start point to build the "right" relationship for interval i. If the interval j doesn't exist, store -1 for the interval i. Finally, you need output the stored value of each interval as an array.

Note:

    You may assume the interval's end point is always bigger than its start point.
    You may assume none of these intervals have the same start point.

**Example 1:**

    Input: [ [1,2] ]

    Output: [-1]

    Explanation: There is only one interval in the collection, so it outputs -1.
 

**Example 2:**

    Input: [ [3,4], [2,3], [1,2] ]

    Output: [-1, 0, 1]

    Explanation: There is no satisfied "right" interval for [3,4].
    For [2,3], the interval [3,4] has minimum-"right" start point;
    For [1,2], the interval [2,3] has minimum-"right" start point.
 

**Example 3:**

    Input: [ [1,4], [2,3], [3,4] ]

    Output: [-1, 2, -1]

    Explanation: There is no satisfied "right" interval for [1,4] and [3,4].
    For [2,3], the interval [3,4] has minimum-"right" start point.

## 442 Find All Duplicates in an Array

Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements that appear twice in this array.

**Could you do it without extra space and in O(n) runtime?**

**Example:**

    Input:
    [4,3,2,7,8,2,3,1]

    Output:
    [2,3]

思路：题目中要求不要使用额外空间和O(n)时间复杂度，所以我们不能使用HashMap或者暴力搜索法。这道题其实思路也算是HashMap，只不过我们使用数组自己本身作为键值对配对。遍历数组中的元素，然后将该数 - 1作为index（因为数组长度正好等于最大数），然后将index对应的原数取负数，继续遍历，若此index对应的数为负，则说明已经出现过。

```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> res = new ArrayList<>();
        
        for(int num : nums){
            int index = Math.abs(num) - 1;
            if(nums[index] < 0){
                res.add(Math.abs(num));
            }else{
                nums[index] = - nums[index];
            }
        }
        return res;
    }
}
```

## 48 Rotate Image
You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

**Example 1:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200831000436.png)

    Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
    Output: [[7,4,1],[8,5,2],[9,6,3]]

**Example 2:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200831000511.png)

    Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
    Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

```java
class Solution {
    public void rotate(int[][] matrix) {
        int N = matrix.length;
        for(int i = 0; i < N; i++){
            for(int j = i; j < N; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        
        for(int i = 0; i < N; i++){
            for(int j = 0; j < N / 2; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][N - 1 - j];
                matrix[i][N - 1 - j] = temp;
            }
        }
    }
}
```
## 7 Reverse Integer
Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

    Input: 123
    Output: 321

**Example 2:**

    Input: -123
    Output: -321

**Example 3:**

    Input: 120
    Output: 21


思路：**先转成int再进行字符串反转** 或者利用下面的数学方法。

看起来这道题就这么解决了，但请注意，题目上还有这么一句

>设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 `[−2^31,  2^31 − 1]`。

也就是说我们不能用`long`存储最终结果，而且有些数字可能是合法范围内的数字，但是反转过来就超过范围了。
假设有`1147483649`这个数字，它是小于最大的32位整数`2147483647`的，但是将这个数字反转过来后就变成了`9463847411`，这就比最大的32位整数还要大了，这样的数字是没法存到int里面的，所以肯定要返回0(溢出了)。
甚至，我们还需要提前判断:

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200831114818.png)

上图中，绿色的是最大32位整数
第二排数字中，橘子的是`5`，它是大于上面同位置的`4`，这就意味着`5`后跟任何数字，都会比最大32为整数都大。
所以，我们到【最大数的1/10】时，就要开始判断了
如果某个数字大于 `214748364`那后面就不用再判断了，肯定溢出了。
如果某个数字等于 `214748364`呢，这对应到上图中第三、第四、第五排的数字，需要要跟最大数的末尾数字比较，如果这个数字比7还大，说明溢出了。

对于负数也是一样的

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200831114859.png)

上图中绿色部分是最小的32位整数，同样是在【最小数的 1/10】时开始判断
如果某个数字小于 `-214748364`说明溢出了
如果某个数字等于 `-214748364`，还需要跟最小数的末尾比较，即看它是否小于8


```java
class Solution {
    public int reverse(int x) {
        int res = 0;
        while(x != 0){
            int tmp = x % 10;
            if (res > Integer.MAX_VALUE / 10 || (res == Integer.MAX_VALUE && tmp>7)) {
                return 0;
            }

            if (res < Integer.MIN_VALUE / 10 || (res == Integer.MIN_VALUE && tmp<-8)) {
                return 0;
            }
            res = res * 10 + tmp;
            x /= 10;
        }
        return res;
    }
}
```

## Valid Anagram
Given two strings s and t , write a function to determine if t is an anagram of s.

**Example 1:**

    Input: s = "anagram", t = "nagaram"
    Output: true

**Example 2:**

    Input: s = "rat", t = "car"
    Output: false

思路：[LeetCode解法](https://leetcode-cn.com/problems/valid-anagram/solution/you-xiao-de-zi-mu-yi-wei-ci-by-leetcode/) 一种是排序，一种是哈希表。

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    char[] str1 = s.toCharArray();
    char[] str2 = t.toCharArray();
    Arrays.sort(str1);
    Arrays.sort(str2);
    return Arrays.equals(str1, str2);
}
```
```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    int[] counter = new int[26];
    for (int i = 0; i < s.length(); i++) {
        counter[s.charAt(i) - 'a']++;
        counter[t.charAt(i) - 'a']--;
    }
    for (int count : counter) {
        if (count != 0) {
            return false;
        }
    }
    return true;
}
```

## 459 Repeated Substring Pattern

Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

 

**Example 1:**

    Input: "abab"
    Output: True
    Explanation: It's the substring "ab" twice.

**Example 2:**

    Input: "aba"
    Output: False

**Example 3:**

    Input: "abcabcabcabc"
    Output: True
    Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int N = s.length();
        for(int i = 1; i <= N / 2; i++){
            if(N % i == 0){
                boolean flag = true;
                for(int j = i; j < N; j++){
                    if(s.charAt(j) != s.charAt(j - i)){
                        flag = false;
                        break;
                    }
                }
                if(flag) return true;
            }
        }
        return false;
    }
}
```
## 763 Partition Labels
A string S of lowercase English letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

 

**Example 1:**

    Input: S = "ababcbacadefegdehijhklij"
    Output: [9,7,8]
    Explanation:
    The partition is "ababcbaca", "defegde", "hijhklij".
    This is a partition so that each letter appears in at most one part.
    A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        int N = S.length();
        int[] last = new int[26];
        List<Integer> res = new ArrayList<>();
        for(int i = 0; i < N; i++){
            last[S.charAt(i) - 'a'] = i;
        }
        int j = 0;
        int now = 0;
        for(int i = 0; i < N; i++){
            j = Math.max(j, last[S.charAt(i) - 'a']);
            if(i == j){
                res.add(i - now + 1);
                now = i + 1;
            }
        }
        return res;
    }
}
```