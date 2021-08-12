# Attention

## 注意事项

- [刷题需要注意的小细节](LeetCode-Attention.md)
- 滑动窗口典型问题：第 3 题、第 76 题、第 438 题
- 双指针典型问题：第 11 题、第 15 题、第 42 题

## 需要注意的一些 API

### java.lang.Character.isLetterOrDigit(int codePoint)

确定指定字符 (Unicode 代码点）是一个字母或数字。
字符被确定是字母或数字，如果不是 isLetter(codePoint) 也不是 isDigit(codePoint) 的字符，则返回 true。

### getOrDefault(Object key, V defaultValue)

Returns the value to which the specified key is mapped, or defaultValue if this map contains no mapping for the key.

### Character.toLowerCase

### Map.Entry<K,V>

A map entry (key-value pair). The Map.entrySet method returns a collection-view of the map, whose elements are of this class. The only way to obtain a reference to a map entry is from the iterator of this collection-view. These Map.Entry objects are valid only for the duration of the iteration; more formally, the behavior of a map entry is undefined if the backing map has been modified after the entry was returned by the iterator, except through the setValue operation on the map entry.

### List 转数组

使用 toArray() 方法
需要特别注意，不能这样写：

```java
ArrayList<String> list=new ArrayList<String>();
String strings[]=(String [])list.toArray();
```

这样写编译没有什么问题，但是运行时会报 ClassCastException，这是因为 Java 中允许向上和向下转型，但是这个转型是否成功是根据 Java 虚拟机中这个对象的类型来实现的。Java 虚拟机中保存了每个对象的类型。而数组也是一个对象。数组的类型是 java.lang.Object。把 java.lang.Object 转换成 java.lang.String 是显然不可能的事情，因为这里是一个向下转型，而虚拟机只保存了这是一个 Object 的数组，不能保证数组中的元素是 String 的，所以这个转型不能成功。数组里面的元素只是元素的引用，不是存储的具体元素，所以数组中元素的类型还是保存在 Java 虚拟机中的。

因此正确的方法是这样的：

```java
//要转换的 list 集合
List<String> testList = new ArrayList<String>(){{add("aa");add("bb");add("cc");}};

//使用 toArray(T[] a) 方法
String[] array2 = testList.toArray(new String[testList.size()]);

//打印该数组
for(int i = 0; i < array2.length; i++){
    System.out.println(array2[i]);
}
```

## 2 Add Two Numbers

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode p = l1, q = l2, cur = res;
        int carry = 0;
  
        if(l1 == null && l2 == null) return null;
  
        while(p != null || q != null){
            int x = 0, y = 0;
            if(p != null) x = p.val;
            if(q != null) y = q.val;
            int sum = carry + x + y;
  
            carry = sum / 10;
  
            cur.next = new ListNode(sum % 10);
            cur = cur.next;
            if(p != null) p = p.next;
            if(q != null) q = q.next;
        }
        if(carry > 0){
            cur.next = new ListNode(carry);
        }
        return res.next;

    }
}
```

## 23 Merge k Sorted Lists

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
    1->4->5,
    1->3->4,
    2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```

Constraints:

```
k == lists.length
0 <= k <= 10^4
0 <= lists[i].length <= 500
-10^4 <= lists[i][j] <= 10^4
lists[i] is sorted in ascending order.
The sum of lists[i].length won't exceed 10^4.
```

关于这个题，我们可以用堆做排序，这时候我们需要一种辅助数据结构-堆，有了堆这个数据结构，难度等级是困难的题目，瞬间变成简单了。
我们把三个链表一股脑的全放到堆里面

```
1->4->5
1->3->4
2->6
```

然后由堆根据节点的 val 自动排好序

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201109234409.png)

这是一个小根堆，我们只需要每次输出堆顶的元素，直到整个堆为空即可。
执行过程如下：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201109234422.png)

```java
class Solution {
	public ListNode mergeKLists(ListNode[] lists) {
		if(lists==null || lists.length==0) {
			return null;
		}
		//创建一个堆，并设置元素的排序方式
		// PriorityQueue<ListNode> queue = new PriorityQueue(new Comparator<ListNode>() {
		// 	public int compare(ListNode o1, ListNode o2) {
		// 		return (o1.val - o2.val);
		// 	}
		// });
        PriorityQueue<ListNode> queue = new PriorityQueue<>((v1, v2) -> v1.val - v2.val);
		//遍历链表数组，然后将每个链表的每个节点都放入堆中
		for(int i=0;i<lists.length;i++) {
			while(lists[i] != null) {
				queue.add(lists[i]);
				lists[i] = lists[i].next;
			}
		}
		ListNode dummy = new ListNode(-1);
		ListNode head = dummy;
		//从堆中不断取出元素，并将取出的元素串联起来
		while( !queue.isEmpty() ) {
			dummy.next = queue.poll();
			dummy = dummy.next;
		}
		dummy.next = null;
		return head.next;
	}
}
```

优化：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201109234458.png)

4 个链表中的最小值，一定来自黄色的部分，黄色的部分就是一个小根堆。
这个堆的元素个数是 k 个，也就是图中的 4 个。
我们建立完 k 个大小的堆后，就不断的从堆中获取节点，如果获取到的节点不为空，即还有下一个节点，那么就将下一个节点放到堆中。利用这个特点我们就可以优化空间了，将原先的 O(N) 的空间复杂度优化到 O(k)。

![](https://pic.leetcode-cn.com/1d4fb6358f39ee7b4ad0b75119352a0fba44c550af0c310d594ae529717cbf3d-5.gif)

```java
class Solution {
	public ListNode mergeKLists(ListNode[] lists) {
		if(lists==null || lists.length==0) {
			return null;
		}
		//创建一个小根堆，并定义好排序函数
		// PriorityQueue<ListNode> queue = new PriorityQueue(new Comparator<ListNode>() {
		// 	public int compare(ListNode o1, ListNode o2) {
		// 		return (o1.val - o2.val);
		// 	}
		// });
        PriorityQueue<ListNode> queue = new PriorityQueue<>((v1, v2) -> v1.val - v2.val);
		ListNode dummy = new ListNode(-1);
		ListNode cur = dummy;
		//这里跟上一版不一样，不再是一股脑全部放到堆中
		//而是只把 k 个链表的第一个节点放入到堆中
		for(int i=0;i<lists.length;i++) {
			ListNode head = lists[i];
			if(head!=null) {
				queue.add(head);
			}
		}
		//之后不断从堆中取出节点，如果这个节点还有下一个节点，
		//就将下个节点也放入堆中
		while(queue.size()>0) {
			ListNode node = queue.poll();
			cur.next = node;
			cur = cur.next;
			if(node.next!=null) {
				queue.add(node.next);
			}
		}
		cur.next = null;
		return dummy.next;
	}
}
```


# TreeMaps

## [729. My Calendar I](https://leetcode.com/problems/my-calendar-i/)

Implement a `MyCalendar` class to store your events. A new event can be added if adding the event will not cause a double booking.

Your class will have the method, `book(int start, int end)`. Formally, this represents a booking on the half open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

A double booking happens when two events have some non-empty intersection (ie., there is some time that is common to both events.)

For each call to the method `MyCalendar.book`, return `true` if the event can be added to the calendar successfully without causing a double booking. Otherwise, return false and do not add the event to the calendar.

Your class will be called like this: `MyCalendar cal = new MyCalendar()`; `MyCalendar.book(start, end)`

**Example 1:**

MyCalendar();
MyCalendar.book(10, 20); // returns true
MyCalendar.book(15, 25); // returns false
MyCalendar.book(20, 30); // returns true

Explanation:
The first event can be booked.  The second can't because time 15 is already booked by another event.
The third event can be booked, as the first event takes every time less than 20, but not including 20.
**Note:**

- The number of calls to`MyCalendar.book` per test case will be at most`1000`.
- In calls to`MyCalendar.book(start, end)`, start and end are integers in the`range [0, 10^9]`.

如果加进来一个，就对所有的books进行一遍遍历，这样效率太低，所以就想到了利用TreeMap的特性。

其中 `ceilingKey()`和 `floorKey()`分别代表返回TreeMap中大于等于/小于等于i的key值。

具体步骤如下：
1.将已存在的books维护成升序排序
2.每次判断curBook是否能插入时，将books从逻辑上划为两部分
左边：左端点小于curBook[0]的books
右边: 左端点大于等于curBooks[0]的books
找到books中第一个左端点大于等于curBook[0]的book，记为right。若right[0] >= curBook[1],那么右边部分一定和curBook没有交集,否则至少和右边第一个有交集

左边同理，找到books中第一个左端点小于curBook[0]的book，记为left，若left[1] < curBook[0]，那左边部门一定和curBook没有交集

只要curBook和左边和右边部门都没交集，则可以插入，否则不能插入，而判断左右分别需要O(logn)的时间

```java
class MyCalendar {
    TreeMap<Integer, Integer> map;

    public MyCalendar() {
        map = new TreeMap<>();  
    }
  
    public boolean book(int start, int end) {
        Integer right = map.ceilingKey(start);
        Integer left = map.floorKey(start);
        if((right == null || right >= end) && (left == null || map.get(left) <= start)){
            map.put(start, end);
            return true;
        }
        return false;
    }
}
```

# TrieTree

## [336. Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/)

Given a list of unique words, return all the pairs of the distinct indices (i, j) in the given list, so that the concatenation of the two words words[i] + words[j] is a palindrome.

**Example 1:**

Input: words = ["abcd","dcba","lls","s","sssll"]
Output: [[0,1],[1,0],[3,2],[2,4]]
Explanation: The palindromes are ["dcbaabcd","abcddcba","slls","llssssll"]
Example 2:

Input: words = ["bat","tab","cat"]
Output: [[0,1],[1,0]]
Explanation: The palindromes are ["battab","tabbat"]
视频解析：

<div style="position: relative; padding: 30% 45%;">
<iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src="https://player.bilibili.com/player.html?aid=886503250&bvid=BV1xK4y1p7MN&cid=294487009&page=1&as_wide=1&high_quality=1&danmaku=0" frameborder="no" scrolling="no" allowfullscreen="true"></iframe>
</div>

```java
class Solution {
    class TrieNode {
        TrieNode[] children = new TrieNode[26];
        int wordIndex = -1;
        List<Integer> restIsPalindrome;
        TrieNode() {
            restIsPalindrome = new ArrayList<>();
        }
    }
  
    TrieNode root = new TrieNode();
    int n;
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> palindromePairs(String[] words) {
        n = words.length;
    
        for (int i = 0; i < n; i++) {
            add(words[i], i);
        }
    
        for (int i = 0; i < n; i++) {
            search(words[i], i);
        }
    
        return res;
    }
  
    private void search(String word, int wordIndex) {
        TrieNode cur = root;
        char[] chs = word.toCharArray();
        for (int i = 0; i < chs.length; i++) {
            int j = chs[i] - 'a';
            if (cur.wordIndex != -1 && isPalindrome(chs, i, chs.length - 1)) {
                res.add(Arrays.asList(wordIndex, cur.wordIndex));
            }
            if (cur.children[j] == null) return;
            cur = cur.children[j];
        }
    
        if (cur.wordIndex != -1 && cur.wordIndex != wordIndex) {
            res.add(Arrays.asList(wordIndex, cur.wordIndex));
        }
    
        for (int j : cur.restIsPalindrome) {
            res.add(Arrays.asList(wordIndex, j));
        }
    }
  
    private void add(String word, int wordIndex) {
        TrieNode cur = root;
        char[] chs = word.toCharArray();
        for (int i = chs.length - 1; i >= 0; i--) {
            int j = chs[i] - 'a';
            if (isPalindrome(chs, 0, i)) {
                cur.restIsPalindrome.add(wordIndex);
            }
        
            if (cur.children[j] == null) {
                cur.children[j] = new TrieNode();
            }
            cur = cur.children[j];
        }
    
        cur.wordIndex = wordIndex;
    }
  
    private boolean isPalindrome(char[] chs, int i, int j) {
        while (i < j) {
            if (chs[i++] != chs[j--]) return false;
        }
    
        return true;
    }
}
```

# Segment Tree (ordinary / zkw)

## [307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

<div style="position: relative; padding: 30% 45%;">
<iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src="https://player.bilibili.com/player.html?aid=798173870&bvid=BV1gy4y1D7dx&cid=268408786&page=1&high_quality=1&danmaku=0" frameborder="no" scrolling="no" allowfullscreen="true"></iframe>
</div>

关于线段树，leetcode中有一道题就是让我们实现一个线段树，就是上面这道题。

一般来说线段树有两种实现方式

- zkw张昆伟线段树
- 普通线段树

### zkw Segment Tree

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210620225046.png)

上图所示的是一棵完美二叉树，叶子数（即数据数组nums的大小4）刚好是2的幂，此时st的大小是2n（算上了无用的st[0]），而且可立即得到第一个叶子结点nums[0]存放在st[4]，第二个叶子节点nums[1]存放在st[5]，以此类推。

**初始化数据数组：**

```java
int n;
int[] st;
public NumArray(int[] nums) {
    n = nums.length;
    st = new int[2 * n];
    for(int i = n; i < 2 * n; i++){
        st[i] = nums[i - n];
    }
    for(int i = n - 1; i >= 1; i--){
        st[i] = st[2 * i] + st[2 * i + 1];
    }    
}

```

**update():**
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210620225923.png)

看图可以发现，当某个元素发生变化时，我们只需要跟着改动该元素即他的向上的各个父结点即可

```java
public void update(int index, int val) {
    // index + n 是为了找到该元素在数据数组中的正确下标
    int diff = val - st[index + n];
    for(int i = index + n; i >= 1; i /= 2){
        st[i] += diff;
    } 
}
```

**rangeSum():**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210620230306.png)

```java
public int sumRange(int left, int right) {
    int res = 0;
    for(int i = left + n, j = right + n; i <= j; i /= 2, j /= 2 ){
        if(i % 2 == 1){
            res += st[i++];
        }
        if(j % 2 == 0){
            res += st[j--];
        } 
    }
    return res;
}
```

### ordinary Segment Tree

Tree based ST

```java
class SegmentTree{
    int[] nums;
    Node root;
    public SegmentTree(int[] nums){
        this.nums = nums;
        this.root = buildTree(nums, 0, numslength - 1);
    }
    private Node buildTree(int[] nums, int start, int end){
        if(start > end) return null;
        Node node = new Node(start, end);
        if(start == end) node.sum = nums[start];
        else{
            int mid = start + (end - start) / 2;
            node.left = buildTree(nums, start, mid);
            node.right = buildTree(nums, mid + 1, end);
            node.sum = node.left.sum + node.right.sum;
        }
        return node;
    }
    public void update(Node node, int i, int val){
        if(node.start == node.end){
            node.sum = val;
            return;
        }
        int mid = node.start + (node.end - node.start) / 2;
        if(i <= mid) update(node.left, i, val);
        else if(i > mid) update(node.right, i, val);
        node.sum - node.left.sum + node.right.sum;
    }
    public int sumRange(Node node, int start, int end){
        if(start > end) return 0;
        if(node.start == srart && node.end == end) return node.sum;
        int mid = node.start + (node.end - node.start) / 2;
        if(end <= mid) return sumRange(node.left, node.right);
        else if(start > mid) return sumRange(node.right, start, end);
        else return sumRange(node.left, start, mid) + sumRange(node.right, mid + 1, end);
    }
}
class Node{
    int start, end, sum;
    Node left, right;
    Node(int start, int end){
        this.start = start;
        this.end = end;
    }
}
```

# Priority Queue / Heap

## [1383. Maximum Performance of a Team](https://leetcode.com/problems/maximum-performance-of-a-team/)

You are given two integers n and k and two integer arrays speed and efficiency both of length n. There are n engineers numbered from 1 to n. `speed[i]` and `efficiency[i]` represent the speed and efficiency of the ith engineer respectively.

Choose at most k different engineers out of the n engineers to form a team with the maximum performance.

The performance of a team is the sum of their engineers' speeds multiplied by the minimum efficiency among their engineers.

Return the maximum performance of this team. Since the answer can be a huge number, return it modulo `1000000000 + 7`.

**Example 1:**

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
Output: 60
Explanation:
We have the maximum performance of the team by selecting engineer 2 (with speed=10 and efficiency=4) and engineer 5 (with speed=5 and efficiency=7). That is, performance = (10 + 5) * min(4, 7) = 60.
开一个2d数组，用来存放speed和efficiency的对应关系，然后根据efficiency从大到小排序。

![](https://pic.leetcode-cn.com/a3b459b5c8401c57a2a5f4f74153b0e2a4df4f94255ef625307f2d3db5e514f4-图片.png)

然后遍历数组，其实这里运用了贪心的算法思想，我们只看当前元素左边的，这样的话每个人作为最低效率时，在其左侧找出至多K - 1个最大速度即可。

其中我们利用Priority Queue（默认小顶堆）来存放speed，这样的好处是当 `pq.size() > k - 1`时，我们就从pq里poll出一个，由于小顶堆的特性，我们poll出的一定是当前speed最小的。

```java
class Solution {
    public int maxPerformance(int n, int[] speed, int[] efficiency, int k) {
        int[][] item = new int[speed.length][2];
        for(int i = 0; i < speed.length; i++){
            item[i][0] = speed[i];
            item[i][1] = efficiency[i];
        }
        Arrays.sort(item, (a, b) -> b[1] - a[1]);
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        long res = 0, sum = 0;
        for(int i = 0 ; i < n ; i++){
            if(queue.size() > k - 1){
                sum -= queue.poll();
            }
            res = Math.max(res, (sum + item[i][0]) * item[i][1]);
            queue.offer(item[i][0]);
            sum += item[i][0];
        }
        return (int)(res % ((int)1e9 + 7));
    }
}
```

# Dynamic Programming

```
一部分详见 Data Structures & Algorithms 中的专题部分
```

## 基本型 I

## 区间型 I

## 区间型 II

### [1690. Stone Game VII](https://leetcode.com/problems/stone-game-vii/)

Alice and Bob take turns playing a game, with **Alice starting first**.

There are n stones arranged in a row. On each player's turn, they can **remove** either the leftmost stone or the rightmost stone from the row and receive points equal to the **sum** of the remaining stones' values in the row. The winner is the one with the higher score when there are no stones left to remove.

Bob found that he will always lose this game (poor Bob, he always loses), so he decided to minimize the score's difference. Alice's goal is to maximize the difference in the score.

Given an array of integers stones where `stones[i]` represents the value of the `ith` stone from the left, return the difference in Alice and Bob's score if they both play optimally.

**Example 1:**

Input: stones = [5,3,1,4,2]
Output: 6
Explanation:

- Alice removes 2 and gets 5 + 3 + 1 + 4 = 13 points. Alice = 13, Bob = 0, stones = [5,3,1,4].
- Bob removes 5 and gets 3 + 1 + 4 = 8 points. Alice = 13, Bob = 8, stones = [3,1,4].
- Alice removes 3 and gets 1 + 4 = 5 points. Alice = 18, Bob = 8, stones = [1,4].
- Bob removes 1 and gets 4 points. Alice = 18, Bob = 12, stones = [4].
- Alice removes 4 and gets 0 points. Alice = 18, Bob = 12, stones = [].
  The score difference is 18 - 12 = 6.
  这道题是典型的区间dp。附上视频。

<div style="position: relative; padding: 30% 45%;">
<iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src="https://player.bilibili.com/player.html?aid=713199646&bvid=BV1eX4y1u7ua&cid=266796910&page=1&as_wide=1&high_quality=1&danmaku=0" frameborder="no" scrolling="no" allowfullscreen="true"></iframe>
</div>

令 `dp[i][j]`表示我方在先手处理 `[i:j]`时可以得到的最多的、领先对手的分差。注意，这里的分差是相对于对手在这个区间内的得分而言。

我方在处理 `[i:j]`区间时就两种选择。

第一种是我方选择第i个元素。这样我方在本轮得分 `sum[i+1:j]`，之后的局势是对方处理 `[i+1:j]`，从递归的角度来看，对手可以在此区间内领先我方的最大分差是 `dp[i+1][j]`。所以回溯到本轮，我方先手处理 `[i:j]`区间能够得到的最大分差就是 `sum[i+1:j]-dp[i+1:j]`。

第二种是我方选择第j个元素，同理我方可以得到的最大分差就是 `sum[i:j-1]-dp[i:j-1]`。

综上，我方会在上面两种方案中选择更优的一种。

最后答案的输出就是 `dp[1:n]`。

```java
class Solution {
    public int stoneGameVII(int[] stones) {
        int n = stones.length;
        int[] preSum = new int[n + 1];
        for(int i = 1; i < n + 1; i++){
            preSum[i] = preSum[i - 1] + stones[i - 1];
        }
        int[][] dp = new int[n + 1][n + 1];
  
        for(int len = 2; len < n + 1; len++){
            for(int i = 1, j = i + len - 1; j < n + 1; i++, j++){
                dp[i][j] = Math.max(preSum[j] - preSum[i] - dp[i + 1][j], preSum[j - 1] - preSum[i - 1] - dp[i][j - 1]);
            }
        }
        return dp[1][n];
    }
}
```

## 53 Maximum Subarray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

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

## 152 Maximum Product Subarray

Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

**Example 1:**

```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2:**

```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

这个题就有很多值得讲的地方了，首先看这个题和上一题 Maximum Subarray 的差异：

- 求乘积的最大值，示例中负数的出现，告诉我们这题和 53 题不一样了，一个正数乘以负数就变成负数，即：**最大值乘以负数就变成了最小值**；
- 因此：**最大值和最小值是相互转换的，这一点提示我们可以把这种转换关系设计到「状态转移方程」里去**；
- 如何解决这个问题呢？这里常见的技巧是在「状态设计」的时候，在原始的状态设计后面多加一个维度，减少分类讨论，降低解决问题的难度。

这里是百度百科的「**无后效性**」词条的解释：

```
无后效性是指如果在某个阶段上过程的状态已知，则从此阶段以后过程的发展变化仅与此阶段的状态有关，而与过程在此阶段以前的阶段所经历过的状态无关。利用动态规划方法求解多阶段决策过程问题，过程的状态必须具备无后效性。
```

再翻译一下就是：「动态规划」通常不关心过程，只关心「阶段结果」，这个「阶段结果」就是我们设计的「状态」。什么算法关心过程呢？「回溯算法」，「回溯算法」需要记录过程，复杂度通常较高。

而将状态定义得更具体，通常来说对于一个问题的解决是满足「无后效性」的。这一点的叙述很理论化，不熟悉朋友可以通过多做相关的问题来理解「无后效性」这个概念。

**第 1 步：状态设计（特别重要）**

- `dp[i][j]`：以`nums[i]` 结尾的连续子数组的最值，计算最大值还是最小值由`j` 来表示，`j` 就两个值；
  - 当`j = 0` 的时候，表示计算的是最小值；
  - 当`j = 1` 的时候，表示计算的是最大值。

这样一来，状态转移方程就容易写出。

**第 2 步：推导状态转移方程（特别重要）**

- 由于状态的设计 `nums[i]` 必须被选取（请大家体会这一点，这一点恰恰好也是使得子数组、子序列问题更加简单的原因：当情况复杂、分类讨论比较多的时候，需要固定一些量，以简化计算）；
- `nums[i]` 的正负和之前的状态值（正负）就产生了联系，由此关系写出状态转移方程：

  - 当`nums[i] > 0` 时，由于是乘积关系：
    - 最大值乘以正数依然是最大值；
    - 最小值乘以同一个正数依然是最小值；
  - 当`nums[i] < 0` 时，依然是由于乘积关系：
    - 最大值乘以负数变成了最小值；
    - 最小值乘以同一个负数变成最大值；
  - 当`nums[i] = 0` 的时候，由于`nums[i]` 必须被选取，最大值和最小值都变成 00 ，合并到上面任意一种情况均成立。
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

## 91 Decode Ways

A message containing letters from A-Z is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number of ways to decode it.

**Example 1:**

```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

这个题其实就是爬楼梯的进阶，只不过边界条件多了一些，需要考虑的多了一点

**第 1 步：定义状态**

既然结尾的字符很重要，在定义状态的时候可以这样定义：

`dp[i]`：以 `s[i]` 结尾的前缀子串有多少种解码方法。

**第 2 步：推导状态转移方程**

根据题意：

如果 `s[i] == '0'` ，字符 `s[i]` 就不能单独解码，所以当 `s[i] != '0'` 时，`dp[i] = dp[i - 1] * 1`。
说明：为了得到长度为 i + 1 的前缀子串的解码个数，需要先得到长度为 i 的解码个数，再对 `s[i]` 单独解码，这里分了两步，根据「分步计数原理」，用乘法。这里的 1 表示乘法单位，语义上表示 `s[i]` 只有 1 种编码。

如果当前字符和它前一个字符，能够解码，即 `10 <= int(s[i - 1..i]) <= 26`，即 `dp[i] += dp[i - 2] * 1`；
说明：不同的解码方法，使用「加法」，理论依据是「分类计数的加法原理」，所以这里用 +=。

注意：状态转移方程里出现了下标 `i - 2`，需要单独处理（如何单独处理，需要耐心调试）。

第 3 步：初始化

如果首字符为 0 ，一定解码不了，可以直接返回 0，非零情况下，`dp[0] = 1`；
第 4 步：考虑输出

输出是 `dp[len - 1]`，符合原始问题。

第 5 步：考虑优化空间

这里当前状态值与前面两个状态有关，因此可以使用三个变量滚动计算。但空间资源一般来说不紧张，不是优化的方向，故不考虑。

```java
class Solution {
    public int numDecodings(String s) {
        int N = s.length();
        if(N == 0) return 0;
  
        int[] dp = new int[N];
  
        if(s.charAt(0) == '0') return 0;
  
        dp[0] = 1;
        for(int i = 1; i < N; i++){
            if((s.charAt(i) - '0') != 0){
                dp[i] = dp[i- 1];
            }
  
            int num = 10 * (s.charAt(i - 1) - '0') + (s.charAt(i) - '0');
            if(num >= 10 && num <= 26){
                if(i == 1){
                    // System.out.println(dp[i]);
                    // System.out.println("----------");
                    dp[i] += dp[i - 1];
  
                }else{
                    dp[i] += dp[i - 2];
                }
            }
        }
        return dp[N - 1];
    }
}
```

## 978 Longest Turbulent Subarray

A subarray A[i], A[i+1], ..., A[j] of A is said to be turbulent if and only if:

- For i <= k < j, A[k] > A[k+1] when k is odd, and A[k] < A[k+1] when k is even;
- OR, for i <= k < j, A[k] > A[k+1] when k is even, and A[k] < A[k+1] when k is odd.

That is, the subarray is turbulent if the comparison sign flips between each adjacent pair of elements in the subarray.

Return the **length** of a maximum size turbulent subarray of A.

**Example 1:**

```
Input: [9,4,2,10,7,8,8,1,9]
Output: 5
Explanation: (A[1] > A[2] < A[3] > A[4] < A[5])
```

**Example 2:**

```
Input: [4,8,12,16]
Output: 2
```

**Example 3:**

```
Input: [100]
Output: 1
```

思路：这个题很不错，既可以用 DP 写，也可以用 Sliding Window 写

```java
class Solution {
    public int maxTurbulenceSize(int[] A) {
        if(A.length==1){
            return 1;
        }
        int[] dp = new int[A.length];
        for(int i=1;i<A.length;i++){
            //如果 i 和 i-1 的值相等，那么 i 位的初始值为 1，譬如 A={9,9}，它返回的长度为 1，而不是 2。
            dp[i] = A[i]==A[i-1]?1:2;
        }
        //初始化 dp 以后，从 2 到 N 去计算最长长度。
        //状态转移方程：dp[i] = dp[i-1] + 1;
        //i 位的可能最大长度可能是：i-1 位上最大长度 + 1（包含 i 自己）
        //那么什么时候可以加上自己算总长度呢，当 i 位和 i-1 位的大小正好跟 i-1 和 i-2 的大小情况相反。说明 i 成功可以加入到前面已经计算的总长度里。
        //否则 i 位就是默认初始化的长度。
        int max = dp[1];
        for(int i=2;i<A.length;i++){
            if(A[i-1]-A[i-2]>0&&A[i]-A[i-1]<0 || A[i-1]-A[i-2]<0&&A[i]-A[i-1]>0){
                dp[i] = dp[i-1] + 1;
            }
            max = Math.max(max,dp[i]);
        }
        return max;
    }
}
```

## 1143 Longest Common Subsequence

Given two strings text1 and text2, return the length of their longest common subsequence.

A subsequence of a string is a new string generated from the original string with some characters(can be none) deleted without changing the relative order of the remaining characters. (eg, "ace" is a subsequence of "abcde" while "aec" is not). A common subsequence of two strings is a subsequence that is common to both strings.

If there is no common subsequence, return 0.

**Example 1:**

```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2:**

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3:**

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

**Constraints:**

```
1 <= text1.length <= 1000
1 <= text2.length <= 1000
The input strings consist of lowercase English characters only.
```

**思路：** 前景提要：[这个视频](https://leetcode-cn.com/problems/longest-common-subsequence/solution/shi-pin-jiang-jie-shi-yong-dong-tai-gui-hua-qiu-ji/) 讲得很不错，可以看一下。
这个题可以通过画表格来分析，会让我们的思路更加清晰，如果当前我们看到两个字符串 `S1[i] == S2[j]`的时候，当前这个 Chaeacter 肯定是要考虑进最长公共子串的，那么当前最长的字串就是 `dp[i - 1][j - 1] + 1`，如果 `S1[i] != S2[j]`，这个当前的 Character 肯定不被考虑，那么当前最长公共字串肯定要在此 Character 之前的两个字符串中找最长的那一个

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200927225640.png)

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int N1 = text1.length();
        int N2 = text2.length();
        int[][] dp = new int[N1 + 1][N2 + 1];
        dp[0][0] = 0;
  
        for(int i = 1; i <= N1; i++){
            for(int j = 1; j <= N2; j++){
                if(text1.charAt(i - 1) == text2.charAt(j - 1)) 
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                else
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[N1][N2];
    }
}
```

## 300 Longest Increasing Subsequence

Given an unsorted array of integers, find the length of longest increasing subsequence.

**Example:**

```
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Note:**

```
There may be more than one LIS combination, it is only necessary for you to return the length.
Your algorithm should run in O(n2) complexity.
```

这是一道动态规划的问题

状态定义：

- `dp[i]` 的值代表`nums` 前 i 个数字的最长子序列长度。
  转移方程： 设`j∈[0,i)`，考虑每轮计算新`dp[i]` 时，遍历`[0,i)` 列表区间，做以下判断：
- 当`nums[i]>nums[j]` 时：`nums[i]` 可以接在`nums[j]` 之后（此题要求严格递增），此情况下最长上升子序列长度为`dp[j] + 1`；
- 当`nums[i]<=nums[j]` 时：`nums[i]` 无法接在`nums[j]` 之后，此情况上升子序列不成立，跳过。
- 上述所有 1. 情况 下计算出的`dp[j] + 1` 的最大值，为直到 ii 的最长上升子序列长度（即`dp[i]` ）。实现方式为遍历 j 时，每轮执行`dp[i] = max(dp[i], dp[j] + 1)`。
- 转移方程：`dp[i] = max(dp[i], dp[j] + 1) for j in [0, i)`。
  初始状态：

`dp[i]` 所有元素置 1，含义是每个元素都至少可以单独成为子序列，此时长度都为 1。
返回值：

返回 dp 列表最大值，即可得到全局最长上升子序列长度。

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 0) return 0;
        int[] dp = new int[nums.length];
        int res = 0;
        Arrays.fill(dp, 1);
        for(int i = 0; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if(nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```

## 516 Longest Palindromic Subsequence

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

**Example 1:**

```
Input:
    "bbbab"
Output:
    4
One possible longest palindromic subsequence is "bbbb".
```

**Example 2:**

```
Input:
    "cbbd"
Output:
    2
One possible longest palindromic subsequence is "bb".
```

**Constraints:**

```
1 <= s.length <= 1000
s consists only of lowercase English letters.
```

![](https://pic.leetcode-cn.com/65ddfb82b07e9d66fad03d34fd5ceb74523e9d93bfea6debe5148b9ed181fcd0-516_5.GIF)

**状态**

`f[i][j]` 表示 s 的第 i 个字符到第 j 个字符组成的子串中，最长的回文序列长度是多少。

**转移方程**

如果 s 的第 i 个字符和第 j 个字符相同的话

`f[i][j] = f[i + 1][j - 1] + 2`

如果 s 的第 i 个字符和第 j 个字符不同的话

`f[i][j] = max(f[i + 1][j], f[i][j - 1])`

然后注意遍历顺序，i 从最后一个字符开始往前遍历，j 从 i + 1 开始往后遍历，这样可以保证每个子问题都已经算好了。

**初始化**

`f[i][i] = 1` 单个字符的最长回文序列是 1

**结果**

`f[0][n - 1]`

关于这个思路，举个例子来想：

```
1、当 s[i] == s[j] 时，考虑 i 和 j 中间序列的奇偶个数， dp[i][j] = dp[i+1][j-1] + 2
    对上述 dp[i][j] =  dp[i+1][j-1] + 2 的解释：
        当序列为 b aa b 时， i = 0, j = 3，则 dp[0][3] = dp[1][2] + 2 = 4
        当序列为 b a b 时，i = 0, j = 2，则 dp[0][2] = dp[1][1] + 2 = 3 
        当序列为 b b 时， i = 0, j = 1，则 dp[0][1] = dp[1][0] = 0 + 2 = 2 (dp[1][0] 默认值为 0)
        该式子同时考虑到了奇偶
2、当 s[i] != s[j] ，那么 dp[i][j] = Math.max(dp[i+1][j],dp[i][j-1])
    对上述 dp[i][j] 式子的解释：
        假如序列为 d c b c c（index：0-4），s[0] != s[4] ，则 dp[0][4] = Math.max(dp[0][3],dp[1,4]) = Math.max(2,3) = 3
```

至于为什么是从后往前遍历的，就是要考虑到： i 从最后一个字符开始往前遍历，j 从 i + 1 开始往后遍历，这样可以保证每个子问题都已经算好了。i 从 0 开始到话，j 要从 i-1 递减到 0 这样遍历，相应到状态方程改成 `f[i][j]=f[i-1][j+1]` or `f[i][j] = max(f[i-1][j], f[i][j+1]`。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] f = new int[n][n];
        for (int i = n - 1; i >= 0; i--) {
            f[i][i] = 1;
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i + 1][j - 1] + 2;
                } else {
                    f[i][j] = Math.max(f[i + 1][j], f[i][j - 1]);
                }
            }
        }
        return f[0][n - 1];
    }
}
```

## [256. Paint House](https://leetcode.com/problems/paint-house/)

There is a row of n houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an n x 3 cost matrix costs.

For example, costs[0][0] is the cost of painting house 0 with the color red; costs[1][2] is the cost of painting house 1 with color green, and so on...
Return the minimum cost to paint all houses.

**Example 1:**

Input: costs = [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
Minimum cost: 2 + 5 + 3 = 10.
动态规划，看图：
![](https://leetcode.com/problems/paint-house/Figures/256/dp_func_call_grid.png)
![](https://leetcode.com/problems/paint-house/Figures/256/dp_in_grid.png)
用这个图来分析，从 index = 1 开始，针对三种颜色，我们分别找出选用当前颜色时，所需要的最少花费：当前花费 + 上一间屋子另外两种颜色的最少花费，这样我们就写好了状态转移方程。

```java
class Solution {
    public int minCost(int[][] costs) {
        int n = costs.length;
        int[][] dp = costs;
        for(int i = 1; i < n; i++){
            dp[i][0] += Math.min(dp[i - 1][1], dp[i - 1][2]);
            dp[i][1] += Math.min(dp[i - 1][0], dp[i - 1][2]);
            dp[i][2] += Math.min(dp[i - 1][1], dp[i - 1][0]);
        }
        return Math.min(dp[n - 1][0], Math.min(dp[n - 1][1], dp[n - 1][2]));
    }
}
```

## [97. Interleaving String](https://leetcode.com/problems/interleaving-string/)

## [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

You are given a 0-indexed integer array nums and an integer k.

You are initially standing at index 0. In one move, you can jump at most k steps forward without going outside the boundaries of the array. That is, you can jump from index i to any index in the range `[i + 1, min(n - 1, i + k)]` inclusive.

You want to reach the last index of the array (index n - 1). Your score is the sum of all nums[j] for each index j you visited in the array.

Return the maximum score you can get.

**Example 1:**

Input: nums = [1,-1,-2,4,-7,3], k = 2
Output: 7
Explanation: You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.
本题初看很像第二类序列型DP，令dp[i]表示跳到第i个位置所能得到的最大得分。很容易写出状态转移方程：

`dp[i] = max(dp[j] + nums[i]) for j=i-k, i-k+1, ... i-1`

这样的话时间复杂度是o(NK)，根据数据范围可以判断会超时。如何改进呢？我们发现，dp[i]的关键是在[i-k,i-1]这个区间里找最大的dp值；类似地，dp[i+1]的关键是在[i-k+1,i]这个区间里找最大的dp值。这两步的两个区间是大部分重叠的，因此应该有高效地方法来分享这些信息，将取区间最大值的操作耗时均摊变小。

显然，这本质就是一个sliding window maximum的问题。我们关注一个长度为k的滑动窗口，里面的最大值就用来更新窗口后的第一个元素的dp值。sliding window maximum的标准解法是用deque，维护一个单调递减的队列。如果有新元素比队尾元素更大，那么它就更有竞争力（更新、更大）被用来更新后面的dp值，故队尾元素就可以舍弃而加入新元素。此外，如果队首元素脱离了这个滑动窗口的范围，也就可以将其舍弃。在每一个回合，deque里面的最大元素就是队首元素。

所以本题最优解的时间复杂度是o(N)

**1.动态规划**

这个是首先想到的
dp思路：dp[i]表示以i为结尾的最大值
对每个dp[i]，都遍历dp(i-k,i-1)找最小值cur
dp[i] = cur + nums[i]

```java
class Solution {
    public int maxResult(int[] nums, int k) {
        int n = nums.length;
        int[] dp = new int[n];
        dp[0] = nums[0];
        for (int i = 1; i < n; i++) {
            int cur = Integer.MIN_VALUE;
            for (int j = Math.max(0, i-k); j < i; j++)
                cur = Math.max(cur, dp[j]);
  
            dp[i] = nums[i] + cur;
        }
        return dp[n-1];
    }
}
```

很遗憾，超时了，下面进行优化

**2.优先队列**

我们建立一个大根堆，这样就可以直接取到最大值，而不需要每次都对dp(i-k,i-1)进行一次遍历找最大值
但要注意，数据范围是流动的，当超范围时，要将移除堆中的数据

```java
class Solution {
    public int maxResult(int[] nums, int k) {
        int n = nums.length;
        Queue<int[]> queue = new PriorityQueue<>((o1, o2) -> o2[0] - o1[0]);
        queue.offer(new int[]{nums[0], 0});
        int res = nums[0];
        for (int i = 1; i < n; i++) {
            while (i - queue.peek()[1] > k)
                queue.poll();

            res = queue.peek()[0] + nums[i];
            queue.offer(new int[]{res, i});
        }
        return res;
    }  
}
```

**3.单调队列**

再次进行优化，上述堆中移除堆顶元素，并重新调整堆仍费时
对这种 动态固定范围找最值的问题 可以采用优先队列

```java
class Solution {
    public int maxResult(int[] nums, int k) {
        int n = nums.length;
        Deque<int[]> queue = new LinkedList<>(); 
        queue.offer(new int[]{nums[0], 0});
        int res = nums[0];
        for (int i = 1; i < n; i++) {
            res = queue.peek()[0] + nums[i];
            while (!queue.isEmpty() && res > queue.peekLast()[0])
                queue.pollLast();
  
            queue.offer(new int[]{res, i});

            while (i - queue.peek()[1] >= k)
                queue.poll();
        }
        return res;
    }  
}
```

优先队列的好处是在于我们不需要手动控制排序的过程，我们事先重写好了排序规则，所以我们只需要手动控制队列的size满足题目要求即可：

```java
while (i - queue.peek()[1] > k)
    queue.poll();
```

但是单调队列由于本质是一个双端队列，我们一方面需要控制队列的size，一方面也要控制队列中的元素是有序的，这个有序用一种更合适的说法就是：确保队列顶部的下一个就是备选的元素，顶部一旦出队列，下一个替补上来就能直接使用：

```java
while (!queue.isEmpty() && res > queue.peekLast()[0])
    queue.pollLast();
  
queue.offer(new int[]{res, i});

while (i - queue.peek()[1] >= k)
    queue.poll();
```

# BackTrack

Sometimes we do not have the obvious revoke step, how can it be called "backtracking" ?

```
a++;
dfs(a);
a--;
```

```
dfs(a++);
```

They are the same !

## Permutation, Combination, Subset

为什么有的时候用 used 数组，有的时候设置搜索起点 begin 变量呢？

排列问题，讲究顺序（即 [2, 2, 3] 与 [2, 3, 2] 视为不同列表时），需要记录哪些数字已经使用过，此时用 used 数组。

组合问题，不讲究顺序（即 [2, 2, 3] 与 [2, 3, 2] 视为相同列表时），需要按照某种顺序搜索，此时使用 start 变量。

- 变量可以重复使用，那么传进递归中的 start = i
- 变量不可以重复使用，那么传进递归中的 start = i + 1

### [46. Permutations](https://leetcode.com/problems/permutations/)

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

**Example 1:**

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
**Example 2:**

Input: nums = [0,1]
Output: [[0,1],[1,0]]
思路：backtracking的典型题目，初始化 `List<List<Integer>> res`用来存放结果，`Deque<Integer> path` 存放单次搜索结果，`boolean[] vis` 保存搜索记录。
唯一注意的一点是，很多题解使用 `int index` 来记录当前遍历的层数，这一参数可有可无，也可以直接用 `path.size()`来进行判断，当作递归终止的条件。

```java
class Solution {
    List<List<Integer>> res;
    Deque<Integer> path;
    boolean[] used;
    public List<List<Integer>> permute(int[] nums) {
        res = new ArrayList<>();
        if(nums == null || nums.length == 0){
            return res;
        }
        path = new ArrayDeque<>();
        used = new boolean[nums.length];
        dfs(nums);
        return res;
    }
    private void dfs(int[] nums){
        if(path.size() == nums.length){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            if(!used[i]){
                path.addLast(nums[i]);
                used[i] = true;
                dfs(nums);
                path.removeLast();
                used[i] = false;
            }
        }
    }
}
```

### [47. Permutations II](https://leetcode.com/problems/permutations-ii/)

Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

**Example 1:**

Input: nums = [1,1,2]
Output:
[[1,1,2],
[1,2,1],
[2,1,1]]
**Example 2:**

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
思路：跟上一题一样，但是多了一点，需要剪枝，因为可能会出现重复的元素。
总体上其实和第一种思路没有太大区别，要注意的是思路：**在一定会产生重复结果集的地方剪枝**。

一个比较容易想到的办法是在结果集中去重。但是问题又来了，这些结果集的元素是一个又一个列表，对列表去重不像用哈希表对基本元素去重那样容易。

如果要比较两个列表是否一样，一个很显然的办法是分别排序，然后逐个比对。既然要排序，我们可以在**搜索之前就对候选数组排序**，一旦发现这一支搜索下去可能搜索到重复的元素就停止搜索，这样结果集中不会包含重复元素。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200717161126.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200717161217.png)

产生重复结点的地方，正是图中标注了“剪刀”，且被绿色框框住的地方。

大家也可以把第 2 个 1 加上 ' ，即 [1, 1', 2] 去想象这个搜索的过程。只要遇到起点一样，就有可能产生重复。这里还有一个很细节的地方：

1. 在图中 ② 处，搜索的数也和上一次一样，但是上一次的 1 还在使用中；
2. **在图中 ① 处，搜索的数也和上一次一样，但是上一次的 1 刚刚被撤销，正是因为刚被撤销，下面的搜索中还会使用到，因此会产生重复，剪掉的就应该是这样的分支。**

所以我们要在代码上加上

```java
if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
    continue;
}
```

```java
class Solution {
    List<List<Integer>> res;
    Deque<Integer> path;
    boolean[] vis;
    public List<List<Integer>> permuteUnique(int[] nums) {
        res = new ArrayList<>();
        path = new ArrayDeque<>();
        vis = new boolean[nums.length];
        Arrays.sort(nums);
        if(nums == null || nums.length == 0){
            return res;
        }
        dfs(nums);
        return res;
  
    }
    private void dfs(int[] nums){
        if(path.size() == nums.length){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            if(vis[i]){
                continue;
            }
            // 剪枝条件：i > 0 是为了保证 nums[i - 1] 有意义
            // 写 !used[i - 1] 是因为 nums[i - 1] 在深度优先遍历的过程中刚刚被撤销选择
            if(i > 0 && nums[i] == nums[i - 1] && !vis[i - 1]){
                continue;
            }
            path.addLast(nums[i]);
            vis[i] = true;
            dfs(nums);
            path.removeLast();
            vis[i] = false;
        }
    }
}
```

### 39 Combination Sum

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is guaranteed that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```

**Example 4:**

```
Input: candidates = [1], target = 1
Output: [[1]]
```

**Example 5:**

```
Input: candidates = [1], target = 2
Output: [[1,1]]
```

**Constraints:**

```
1 <= candidates.length <= 30
1 <= candidates[i] <= 200
All elements of candidates are distinct.
1 <= target <= 500
```

以输入：`candidates = [2, 3, 6, 7]`, `target = 7` 为例：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201002231223.png)

**说明：**

- 以`target = 7` 为 根结点 ，创建一个分支的时 做减法 ；
- 每一个箭头表示：从父亲结点的数值减去边上的数值，得到孩子结点的数值。边的值就是题目中给出的 candidate 数组的每个元素的值；
- 减到 00 或者负数的时候停止，即：结点 00 和负数结点成为叶子结点；
- 所有从根结点到结点 00 的路径（只能从上往下，没有回路）就是题目要找的一个结果。

这棵树有 44 个叶子结点的值 00，对应的路径列表是 `[[2, 2, 3], [2, 3, 2], [3, 2, 2], [7]]`，而示例中给出的输出只有 [[7], [2, 2, 3]]。即：题目中要求每一个符合要求的解是 不计算顺序 的。下面我们分析为什么会产生重复。

**针对具体例子分析重复路径产生的原因（难点）**

产生重复的原因是：在每一个结点，做减法，展开分支的时候，由于题目中说 **每一个元素可以重复使用**，我们考虑了 **所有的** 候选数，因此出现了重复的列表。

一种简单的去重方案是借助哈希表的天然去重的功能，但实际操作一下，就会发现并没有那么容易。

可不可以在搜索的时候就去重呢？答案是可以的。遇到这一类相同元素不计算顺序的问题，我们在搜索的时候就需要 **按某种顺序搜索**。具体的做法是：每一次搜索的时候设置 **下一轮搜索的起点** begin，请看下图。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201002231424.png)

即：从每一层的第 2 个结点开始，都不能再搜索产生同一层结点已经使用过的 candidate 里的元素。

我用的方法：

```java
class Solution {
    List<List<Integer>> res;
    int sum;
    Deque<Integer> path;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        res = new ArrayList<>();
        path = new ArrayDeque<>();
        if(candidates == null || candidates.length == 0){
            return res;
        }
        dfs(candidates, target, 0);
        return res;
  
    }
    private void dfs(int[] candidates, int target, int start){
        if(sum == target){
            res.add(new ArrayList<>(path));
            return;
        }
        if(sum > target) return;
        for(int i = start; i < candidates.length; i++){
            path.addLast(candidates[i]);
            sum += candidates[i];
            dfs(candidates, target, i);
            sum -= candidates[i];
            path.removeLast();
        }  
    }
}
```

题解方法：

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        int len = candidates.length;
        List<List<Integer>> res = new ArravyList<>();
        if (len == 0) {
            return res;
        }

        Deque<Integer> path = new ArrayDeque<>();
        dfs(candidates, 0, len, target, path, res);
        return res;
    }

    /**
     * @param candidates 候选数组
     * @param begin      搜索起点
     * @param len        冗余变量，是 candidates 里的属性，可以不传
     * @param target     每减去一个元素，目标值变小
     * @param path       从根结点到叶子结点的路径，是一个栈
     * @param res        结果集列表
     */
    private void dfs(int[] candidates, int begin, int len, int target, Deque<Integer> path, List<List<Integer>> res) {
        // target 为负数和 0 的时候不再产生新的孩子结点
        if (target < 0) {
            return;
        }
        if (target == 0) {
            res.add(new ArrayList<>(path));
            return;
        }

        // 重点理解这里从 begin 开始搜索的语意
        for (int i = begin; i < len; i++) {
            path.addLast(candidates[i]);

            // 注意：由于每一个元素可以重复使用，下一轮搜索的起点依然是 i，这里非常容易弄错
            dfs(candidates, i, len, target - candidates[i], path, res);

            // 状态重置
            path.removeLast();
        }
    }
}
```

改进：剪枝

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        int len = candidates.length;
        List<List<Integer>> res = new ArrayList<>();
        if (len == 0) {
            return res;
        }

        // 排序是剪枝的前提
        Arrays.sort(candidates);
        Deque<Integer> path = new ArrayDeque<>();
        dfs(candidates, 0, len, target, path, res);
        return res;
    }

    private void dfs(int[] candidates, int begin, int len, int target, Deque<Integer> path, List<List<Integer>> res) {
        // 由于进入更深层的时候，小于 0 的部分被剪枝，因此递归终止条件值只判断等于 0 的情况
        if (target == 0) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = begin; i < len; i++) {
            // 重点理解这里剪枝，前提是候选数组已经有序，
            if (target - candidates[i] < 0) {
                break;
            }
  
            path.addLast(candidates[i]);
            dfs(candidates, i, len, target - candidates[i], path, res);
            path.removeLast();
        }
    }
}
```

### 40 Combination Sum II

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

Each number in candidates may only be used once in the combination.

**Note:**

- All numbers (including target) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
[1, 7],
[1, 2, 5],
[2, 6],
[1, 1, 6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
[1,2,2],
[5]
]
```

这道题与上一问的区别在于：

```
第 39 题：candidates 中的数字可以无限制重复被选取；
第 40 题：candidates 中的每个数字在每个组合中只能使用一次。
```

相同点是：相同数字列表的不同排列视为一个结果。

**如何去掉重复的集合（重点）**

为了使得解集不包含重复的组合。有以下 22 种方案：

1. 使用 哈希表 天然的去重功能，但是编码相对复杂；
2. 这里我们使用和第 39 题和第 15 题（三数之和）类似的思路：不重复就需要按 顺序 搜索， 在搜索的过程中检测分支是否会出现重复结果 。注意：这里的顺序不仅仅指数组 candidates 有序，还指按照一定顺序搜索结果。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201003000606.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201003000613.png)

由第 39 题我们知道，数组 candidates 有序，也是 深度优先遍历 过程中实现「剪枝」的前提。
将数组先排序的思路来自于这个问题：去掉一个数组中重复的元素。很容易想到的方案是：先对数组 升序 排序，重复的元素一定不是排好序以后相同的连续数组区域的第 11 个元素。也就是说，剪枝发生在：同一层数值相同的结点第 22、33 ... 个结点，因为数值相同的第 11 个结点已经搜索出了包含了这个数值的全部结果，同一层的其它结点，候选数的个数更少，搜索出的结果一定不会比第 11 个结点更多，并且是第 11 个结点的子集。（说明：这段文字很拗口，大家可以结合具体例子，在纸上写写画画进行理解。）

我用的方法：

```java
class Solution {
    List<List<Integer>> res;
    Deque<Integer> path;
    int sum = 0;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        res = new ArrayList<>();
        path = new ArrayDeque<>();
        Arrays.sort(candidates);
        dfs(candidates, target, 0);
        return res;
  
    }
    private void dfs(int[] candidates, int target, int start){
        if(sum == target){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = start; i < candidates.length; i++){
            if(sum > target){
                return;
            }
            if(i > start && candidates[i] == candidates[i - 1]){
                continue;
            }
            path.addLast(candidates[i]);
            sum += candidates[i];
            dfs(candidates, target, i + 1);
            sum -= candidates[i];
            path.removeLast();
        }
    }
}
```

题解的方法：

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        int len = candidates.length;
        List<List<Integer>> res = new ArrayList<>();
        if (len == 0) {
            return res;
        }

        // 关键步骤
        Arrays.sort(candidates);

        Deque<Integer> path = new ArrayDeque<>(len);
        dfs(candidates, len, 0, target, path, res);
        return res;
    }

    /**
     * @param candidates 候选数组
     * @param len        冗余变量
     * @param begin      从候选数组的 begin 位置开始搜索
     * @param target     表示剩余，这个值一开始等于 target，基于题目中说明的"所有数字（包括目标数）都是正整数"这个条件
     * @param path       从根结点到叶子结点的路径
     * @param res
     */
    private void dfs(int[] candidates, int len, int begin, int target, Deque<Integer> path, List<List<Integer>> res) {
        if (target == 0) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = begin; i < len; i++) {
            // 大剪枝：减去 candidates[i] 小于 0，减去后面的 candidates[i + 1]、candidates[i + 2] 肯定也小于 0，因此用 break
            if (target - candidates[i] < 0) {
                break;
            }

            // 小剪枝：同一层相同数值的结点，从第 2 个开始，候选数更少，结果一定发生重复，因此跳过，用 continue
            if (i > begin && candidates[i] == candidates[i - 1]) {
                continue;
            }

            path.addLast(candidates[i]);
            // 调试语句 ①
            // System.out.println("递归之前 => " + path + "，剩余 = " + (target - candidates[i]));

            // 因为元素不可以重复使用，这里递归传递下去的是 i + 1 而不是 i
            dfs(candidates, len, i + 1, target - candidates[i], path, res);

            path.removeLast();
            // 调试语句 ②
            // System.out.println("递归之后 => " + path + "，剩余 = " + (target - candidates[i]));
        }
    }

    public static void main(String[] args) {
        int[] candidates = new int[]{10, 1, 2, 7, 6, 1, 5};
        int target = 8;
        Solution solution = new Solution();
        List<List<Integer>> res = solution.combinationSum2(candidates, target);
        System.out.println("输出 => " + res);
    }
}
```

解释语句：`if cur > begin and candidates[cur-1] == candidates[cur]` 是如何避免重复的：

```
这个避免重复当思想是在是太重要了。
这个方法最重要的作用是，可以让同一层级，不出现相同的元素。即
                 1
                / \
               2   2  这种情况不会发生 但是却允许了不同层级之间的重复即：
              /     \
             5       5
                例 2
                 1
                /
               2      这种情况确是允许的
              /
             2  
  
为何会有这种神奇的效果呢？
首先 cur-1 == cur 是用于判定当前元素是否和之前元素相同的语句。这个语句就能砍掉例 1。
可是问题来了，如果把所有当前与之前一个元素相同的都砍掉，那么例二的情况也会消失。 
因为当第二个 2 出现的时候，他就和前一个 2 相同了。
  
那么如何保留例 2 呢？
那么就用 cur > begin 来避免这种情况，你发现例 1 中的两个 2 是处在同一个层级上的，
例 2 的两个 2 是处在不同层级上的。
在一个 for 循环中，所有被遍历到的数都是属于一个层级的。我们要让一个层级中，
必须出现且只出现一个 2，那么就放过第一个出现重复的 2，但不放过后面出现的 2。
第一个出现的 2 的特点就是 cur == begin. 第二个出现的 2 特点是 cur > begin.
```

### 216 Combination Sum III

Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Note:**

- All numbers will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

树的 dfs 从上往下开始执行的时候因为递归分为递和归两部分（也就是往下传递和往回走），来看一个简单的例子，比如阶乘的递归过程，是先往下传递，然后再往回走

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200913162606.png)

所以上面代码也一样，往下传递的时候我们要把当前节点的值加入到一个集合中，并且用 n 减去当前节点的值，返回的时候再把它给移除掉就行了。那么终止条件是什么呢，就是集合中的 size 等于 k，并且 n 等于 0，搞懂了上面的过程，代码就呼之欲出了

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        Deque<Integer> path = new ArrayDeque<>();
        dfs(k, n, 1, res, path);
        return res;
    }
  
    private void dfs(int k, int n, int start, List<List<Integer>> res, Deque<Integer> path){
        if(path.size() == k && n == 0){
            res.add(new ArrayList<>(path));
            return;
        }
  
        for(int i = start; i <= 9; i++){
            path.addLast(i);
            dfs(k, n - i, i + 1, res, path);
            path.removeLast();
        }
    }
}
```

### [78. Subsets](https://leetcode.com/problems/subsets/)

Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Example 1:**

Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
回溯，做到这里其实一目了然了，首先子集，肯定用begin变量，因为不可以重复。

其次因为是子集，没有特殊条件，所以dfs func里面直接 `res.add(new ArrayList<>(path))`即可

```java
class Solution {
    List<List<Integer>> res;
    Deque<Integer> path;
    public List<List<Integer>> subsets(int[] nums) {
        res = new ArrayList<>();
        path = new ArrayDeque<>();
        dfs(nums, 0);
        return res;
  
    }
    private void dfs(int[] nums, int start){
        res.add(new ArrayList<>(path));
        for(int i = start; i < nums.length; i++){
            path.addLast(nums[i]);
            dfs(nums, i + 1);
            path.removeLast();
        }
    }
}
```

### [90. Subsets II](https://leetcode.com/problems/subsets-ii/)

Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Example 1:**

Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
跟上个题目多了一点，就是元素可能会有重复，思路不变，加一个判断即可：

```
if(i > start && nums[i] == nums[i - 1]){
    continue;
}
```

```java
class Solution {
    List<List<Integer>> res;
    Deque<Integer> path;
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        res = new ArrayList<>();
        path = new ArrayDeque<>();
        Arrays.sort(nums);
        dfs(nums, 0);
        return res;
  
    }
    private void dfs(int[] nums, int start){
        res.add(new ArrayList<>(path));
        for(int i = start; i < nums.length; i++){
            if(i > start && nums[i] == nums[i - 1]){
                continue;
            }
            path.addLast(nums[i]);
            dfs(nums, i + 1);
            path.removeLast();
        }
    }
}
```

## Backtacking in String

### [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

**Example 1:**

Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
**Example 2:**

Input: n = 1
Output: ["()"]
这里如果是严格按照「回溯法」的定义去做，是这样写的。大家可以比对一下，与直接使用字符串拼接在实现细节上的不同。

在强调一下重点：「回溯算法」强调了在状态空间特别大的时候，只用一份状态变量去搜索所有可能的状态，在搜索到符合条件的解的时候，通常会做一个拷贝，这就是为什么经常在递归终止条件的时候，有 res.add(new ArrayList<>(path)); 这样的代码。正是因为全程使用一份状态变量，因此它就有「恢复现场」和「撤销选择」的需要。

```java
class Solution {
    List<String> res = new ArrayList<>();
    String path = "";
    public List<String> generateParenthesis(int n) {
        if(n == 0){
            return res;
        }
        dfs(n, 0, 0);
        return res;
    }
    private void dfs(int n, int left, int right){
        if(left == n && right == n){
            res.add(path);
            return;
        }
        if(left < right){
            return;
        }
        if(left < n){
            path = path + "(";
            dfs(n, left + 1, right);
            path = path.substring(0, path.length() - 1);
        }
        if(right < n){
            path = path + ")";
            dfs(n, left, right + 1);
            path = path.substring(0, path.length() - 1);
        }
    }
}
```

当然这个题也可以不用显示的回溯

![](https://pic.leetcode-cn.com/efbe574e5e6addcd1c9dc5c13a50c6f162a2b14a95d6aed2c394e18287a067fa-image.png)

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {

    // 做加法

    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        // 特判
        if (n == 0) {
            return res;
        }

        dfs("", 0, 0, n, res);
        return res;
    }

    /**
     * @param curStr 当前递归得到的结果
     * @param left   左括号已经用了几个
     * @param right  右括号已经用了几个
     * @param n      左括号、右括号一共得用几个
     * @param res    结果集
     */
    private void dfs(String curStr, int left, int right, int n, List<String> res) {
        if (left == n && right == n) {
            res.add(curStr);
            return;
        }

        // 剪枝
        if (left < right) {
            return;
        }

        if (left < n) {
            dfs(curStr + "(", left + 1, right, n, res);
        }
        if (right < n) {
            dfs(curStr + ")", left, right + 1, n, res);
        }
    }
}
```

到最后其实可以发现，主要是跟字符串的特点有关，Java 和 Python 里 + 生成了新的字符串，每次往下面传递的时候，都是新字符串。因此在搜索的时候不用回溯。

### [784. Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)

Given a string s, we can transform every letter individually to be lowercase or uppercase to create another string.

Return a list of all possible strings we could create. You can return the output in any order.

**Example 1:**

Input: s = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]
**Example 2:**

Input: s = "3z4"
Output: ["3z4","3Z4"]
这个题其实也是回溯的思路，难点有两个：

1. 如何小写字母（大写字母）转 大写字母（小写字母）
   - `Character.toLowerCase()` /`Character.toUpperCase()`
   - 位运算：`^= 1 << 5`
2. 因为是对每一位都做转换处理，所以一个字符串有很多种处理结果，**这就导致我们递归的时候还要在判断字母大小写的条件外面再来个递归**

下面的是比较笨拙的写法

```java
class Solution {
    List<String> res = new ArrayList<>();
    StringBuilder sb;
    int len;
    public List<String> letterCasePermutation(String s) {
        sb = new StringBuilder(s);
        len = s.length();
        dfs(0);
        return res;
    }
    private void dfs(int index){
        System.out.println(index);
        if(index == len){
            res.add(sb.toString());
            return;
        }
        char c = sb.charAt(index);
        dfs(index + 1);
        if(Character.isUpperCase(c)){
            sb.setCharAt(index, Character.toLowerCase(c));
            dfs(index + 1);
            sb.setCharAt(index, c);
        }
        if(Character.isLowerCase(c)){
            sb.setCharAt(index, Character.toUpperCase(c));
            dfs(index + 1);
            sb.setCharAt(index, c);
        }
    }
}
```

如果用位运算的话，代码会更加简洁，而且高效。

```java
public class Solution {

    public List<String> letterCasePermutation(String S) {
        List<String> res = new ArrayList<>();
        char[] charArray = S.toCharArray();
        dfs(charArray, 0, res);
        return res;
    }

    private void dfs(char[] charArray, int index, List<String> res) {
        if (index == charArray.length) {
            res.add(new String(charArray));
            return;
        }

        dfs(charArray, index + 1, res);
        if (Character.isLetter(charArray[index])) {
            charArray[index] ^= 1 << 5;
            dfs(charArray, index + 1, res);
        }
    }
}
```

### [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example 1:**

Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
注意更便捷的HashMap初始化方式。

```java
class Solution {
    Map<Character, String> map = Map.of(
        '2', "abc", '3', "def", '4', "ghi", '5', "jkl",
        '6', "mno", '7', "pqrs", '8', "tuv", '9', "wxyz"
    );
    List<String> res = new ArrayList<>();
    String path = "";
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0 || digits == null){
            return res;
        }
        dfs(digits, 0);
        return res;
    
    }
    private void dfs(String digits, int index){
        if(index == digits.length()){
            res.add(path);
            return;
        }
        char c = digits.charAt(index);
        String s = map.get(c);
        for(int j = 0; j < s.length(); j++){
            char option = s.charAt(j);
            path = path + option;
            dfs(digits, index + 1);
            path = path.substring(0, path.length() - 1);
        }    
    }
}
```

### [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

Given the root of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
这个题用回溯，没毛病。
需要注意的是两点：

1. 递归结束的处理，一般情况下我们都是`node == null`的时候写终止处理逻辑，但是这个题其实是要在找到叶子结点的时候就要开始处理结束逻辑了，所以我们需要在`node.left == null && node.right == null`的时候就要把`path`放进结果集里面。
2. revoke for every recursion steps (`dfs(node.left) && dfs(node.right)`)

```java
class Solution {
    List<String> res = new ArrayList<>();
    StringBuilder sb = new StringBuilder();
    public List<String> binaryTreePaths(TreeNode root) {
        if(root == null){
            return res;
        }
        dfs(root);
        return res;
    
    }
    private void dfs(TreeNode node){
        if(node == null){
            return;
        }
        sb.append(node.val);
        if(node.left == null && node.right == null){
            res.add(sb.toString());
        }
        sb.append("->");
        int size = sb.length();
        dfs(node.left);
        sb.delete(size, sb.length());
        dfs(node.right);
        sb.delete(size, sb.length());


    }
}
```

## Game Question

### [51. N-Queens](https://leetcode.com/problems/n-queens/)

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Example 1:**

Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
N Queen就是保证上下左右对角线都不能有棋子，这个问题是典型的dfs+回溯，我们需要考虑的是用什么方法更加方便的判断对角线上的棋子。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200722220353.png)

main diagonal, `x - y = fixed value`, sub diagonal, `x + y = fixed value`.

for the main diagonal, cuz we want to use an array to store the states, but there will be cases where the difference is negative, so we need to `x - y + n - 1` in order to match the index.

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    Deque<Integer> path = new ArrayDeque<>();
    boolean[] col;
    boolean[] main;
    boolean[] sub;
    public List<List<String>> solveNQueens(int n) {
        col = new boolean[n];
        main = new boolean[2 * n - 1];
        sub = new boolean[2 * n - 1];
        dfs(n, 0);
        return res;
    
    }
    private void dfs(int n, int row){
        if(row == n){
            List<String> nPath = new ArrayList<>();
            for(int num : path){
                String nr = "";
                for(int i = 0; i < n; i++){
                    if(i == num){
                        nr += "Q";
                    }else{
                        nr += ".";
                    }
                }
                nPath.add(nr);
            }
            res.add(nPath);
            return;
        }
        for(int i = 0; i < n; i++){
            if(col[i] || main[row - i + n - 1] || sub[row + i]){
                continue;
            }
            path.addLast(i);
            col[i] = true;
            main[row - i + n - 1] = true;
            sub[row + i] = true;
            dfs(n, row + 1);
            sub[row + i] = false;
            main[row - i + n - 1] = false;
            col[i] = false;
            path.removeLast();
        }
    }
}
```

### [52. N-Queens II](https://leetcode.com/problems/n-queens-ii/)

same as 51. N-Queens, even simpler than last one, we only need to use a variable `res` to store the result.

```java
class Solution {
    boolean[] col;
    boolean[] main;
    boolean[] sub;
    int res = 0;
    public int totalNQueens(int n) {
        col = new boolean[n];
        main = new boolean[2 * n - 1];
        sub = new boolean[2 * n - 1]; 
        dfs(n, 0);
        return res;
    }
    private void dfs(int n, int row){
        if(row == n){
            res++;
            return;
        }
        for(int i = 0; i < n; i++){
            if(col[i] || main[row - i + n - 1] || sub[row + i]){
                continue;
            }
            col[i] = true;
            main[row - i + n - 1] = true;
            sub[row + i] = true;
            dfs(n, row + 1);
            sub[row + i] = false;
            main[row - i + n - 1] = false;
            col[i] = false;
        }
    }
}
```

### [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

Given an `m x n` grid of characters `boarWrite a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

Each of the digits 1-9 must occur exactly once in each row.
Each of the digits 1-9 must occur exactly once in each column.
Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
The '.' character indicates empty cells.

Example 1:

Input: board =
[["5","3",".",".","7",".",".",".","."],
["6",".",".","1","9","5",".",".","."],
[".","9","8",".",".",".",".","6","."],
["8",".",".",".","6",".",".",".","3"],
["4",".",".","8",".","3",".",".","1"],
["7",".",".",".","2",".",".",".","6"],
[".","6",".",".",".",".","2","8","."],
[".",".",".","4","1","9",".",".","5"],
[".",".",".",".","8",".",".","7","9"]]

Output:
[["5","3","4","6","7","8","9","1","2"],
["6","7","2","1","9","5","3","4","8"],
["1","9","8","3","4","2","5","6","7"],
["8","5","9","7","6","1","4","2","3"],
["4","2","6","8","5","3","7","9","1"],
["7","1","3","9","2","4","8","5","6"],
["9","6","1","5","3","7","2","8","4"],
["2","8","7","4","1","9","6","3","5"],
["3","4","5","2","8","6","1","7","9"]]

Explanation: The input board is shown above and the only valid solution is shown below:
d `and a string`word `, return `true `if`word` exists in the grid.
in order to reduce the time complexity, we could **set the return value of backtrack func as boolean**, once there is valid solution, return true. this could help to stop the following recursion.

**2 methods of base case:**

```java
if(y == 9){
    y = 0;
    x++;
    if(x == 9){
        return true;
    }
}
```

```java
if(x == 9) return true;
if(y == 9) return dfs(board, x + 1, 0);
```

Solution:

```java
class Solution {
    boolean[][] col; 
    boolean[][] row;
    boolean[][][] sub;
    public void solveSudoku(char[][] board) {
        col = new boolean[9][10];
        row = new boolean[9][10];
        sub = new boolean[3][3][10];
        for(int x = 0; x < board.length; x++){
            for(int y = 0; y < board[0].length; y++) {
                int num = board[x][y] - '0';
                if(1 <= num && num <= 9){
                    row[x][num] = true;
                    col[y][num] = true;
                    sub[x/3][y/3][num] = true;
                }
            }
        }
        dfs(board, 0, 0);
    
    }
    private boolean dfs(char[][] board, int x, int y){
        if(y == 9){
            y = 0;
            x++;
            if(x == 9){
                return true;
            }
        }

        if(board[x][y] != '.'){
            return dfs(board, x, y + 1);
        }else{
            for(int i = 1; i < 10; i++){
                if(col[y][i] || row[x][i] || sub[x / 3][y / 3][i]){
                    continue;
                }
                board[x][y] = (char)('0' + i);
                col[y][i] = true;
                row[x][i] = true;
                sub[x / 3][y /3][i] = true;
                if(dfs(board, x, y + 1)){
                    return true;
                }
                sub[x / 3][y / 3][i] = false;
                row[x][i] = false;
                col[y][i] = false;
                board[x][y] = '.';
            }
        }

        return false;
    }
}
```

another solution

```java
class Solution {
    public void solveSudoku(char[][] board) {
        help(board, 0, 0);
    }
  
    public boolean help(char[][] board, int i, int j){
        if(i == 9) return true;
        if(j == 9) return help(board, i + 1, 0);
        if(board[i][j] != '.') return help(board,i, j + 1);
    
        for(char c = '1'; c <= '9'; c++){
            if(!isValid(board, i, j, c)){
                continue;
            }
            board[i][j] = c;
            if(help(board, i, j + 1)){
                return true;
            }
            board[i][j] = '.';
        }
        return false;
    }
  
    public boolean isValid(char[][] board, int row, int col, char c){
        for(int i = 0; i < 9; i++){
            if(board[i][col] == c){
                return false;
            }
            if(board[row][i] == c){
                return false;
            }
            if(board[( row / 3 ) * 3 + i / 3][(col / 3) * 3 + i % 3] == c){
               return false; 
            }
        }
        return true;
    }
}

```

## [79. Word Search](https://leetcode.com/problems/word-search/)

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**
![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

```java
class Solution {
    int m;
    int n;
    int[][] dir = {{1, 0},{-1 ,0},{0, 1},{0, -1}};
    public boolean exist(char[][] board, String word) {
        m = board.length;
        n = board[0].length;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(dfs(board, i, j, 0, word)){
                    return true;
                }
            }
        }
        return false;
    }
    private boolean dfs(char[][] board, int x, int y, int index, String word){
        if(index == word.length()){
            return true;
        }
        if(x < 0 || x > m - 1 || y < 0 || y > n - 1){
            return false;
        }
        if(board[x][y] == '*' || board[x][y] != word.charAt(index)){
            return false;
        }
        board[x][y] = '*';
        for(int[] d : dir){
            int nx = x + d[0];
            int ny = y + d[1];
            if(dfs(board, nx, ny, index + 1, word)){
                return true;
            }
        }
        board[x][y] = word.charAt(index);
        return false;
    }
}
```

## [212. Word Search II](https://leetcode.com/problems/word-search-ii/)

Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
思路：这个题如果单纯的使用 dfs，时间一定会超出限制，所以我们采用 TrieTree（字典树）来优化。关于这个题，B 站这个老姐讲的特别好：

<div style="position: relative; padding: 30% 45%;">
<iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src="https://player.bilibili.com/player.html?aid=67271723&bvid=BV1TJ411K7hM&cid=116642810&page=1&as_wide=1&high_quality=1&danmaku=0" frameborder="no" scrolling="no" allowfullscreen="true"></iframe>
</div>

```java
class TrieNode{
    String word;
    TrieNode[] children;
    public TrieNode(){
        children = new TrieNode[26];
        word = null;
    }
}
class Solution {
    int[][] dir = {{1, 0},{-1, 0},{0, 1},{0, -1}};
    List<String> res;
    int m;
    int n;
    public List<String> findWords(char[][] board, String[] words) {
        res = new ArrayList<>();
        m = board.length;
        n = board[0].length;
        TrieNode root = new TrieNode();
        buildTrie(words, root);
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                char c = board[i][j]; 
                if(root.children[c - 'a'] != null){
                    dfs(board, i, j, root);
                }
            }
        }
        return res;  
    }
    private void dfs(char[][] board, int x, int y, TrieNode cur){
  
        if(x < 0 || x > m - 1 || y < 0 || y > n - 1){
            return;
        }
        char c = board[x][y];
        if( c == '*' || cur.children[c - 'a'] == null){
            return;
        }
        cur = cur.children[c - 'a'];
        if(cur.word != null){
            res.add(cur.word);
            cur.word = null;
        }
        board[x][y] = '*';
        for(int[] d : dir){
            int nx = x + d[0];
            int ny = y + d[1];
            dfs(board, nx, ny, cur);
        }
        board[x][y] = c;
  
    }
    private void buildTrie(String[] words, TrieNode root){
        for(String s : words){
            TrieNode cur = root;
            for(char c : s.toCharArray()){
                if(cur.children[c - 'a'] == null){
                    cur.children[c - 'a'] = new TrieNode();
                }
                cur = cur.children[c - 'a'];
            }
            cur.word = s;
        }
    }
}
```

## [1059. All Paths from Source Lead to Destination](https://leetcode.com/problems/all-paths-from-source-lead-to-destination/)

Given the edges of a directed graph where `edges[i] = [ai, bi]` indicates there is an edge between nodes `ai` and `bi`, and two nodes source and destination of this graph, determine whether or not all paths starting from source eventually, end at destination, that is:

- At least one path exists from the source node to the destination node
- If a path exists from the source node to a node with no outgoing edges, then that node is equal to destination.
- The number of possible paths from source to destination is a finite number.
  Return true if and only if all roads from source lead to destination.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/03/16/485_example_1.png)

```
Input: n = 3, edges = [[0,1],[0,2]], source = 0, destination = 2
Output: false
Explanation: It is possible to reach and get stuck on both node 1 and node 2.
```

**Graph + DFS + backtracking**

1. construct the graph:`Map<Integer, List<Integer>> map`
2. `boolean[] visted` to determine whether the current node was visted
3. base case of dfs:
   1. determine whether the current node is concluded in the map, if not, which means cur node does not have out degrees, it is the end
   2. determine cur node == destination ? return true : return false

**this question allows us to return true if and only if all roads from source lead to destination. so we need to dfs all the possible result and if all the results meet the conditions, we could return true, otherwise return false**

so we should return true after all the false conditions!!!

```java
class Solution {
    public boolean leadsToDestination(int n, int[][] edges, int source, int destination) {
        // build the graph
        HashMap<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] edge : edges) {
            graph.putIfAbsent(edge[0], new ArrayList<>());
            graph.get(edge[0]).add(edge[1]);
        }
        return helper(graph, new HashSet<>(), source, destination);
    }

    private boolean helper(Map<Integer, List<Integer>> graph, Set<Integer> visited, int cur, int end) {
        // base case
        if (!graph.containsKey(cur)) {
            return cur == end;
        }
        visited.add(cur);
        for (int neighbor : graph.get(cur)) {
            if (visited.contains(neighbor) || !helper(graph, visited, neighbor, end)) {
                return false;
            }
        }
        visited.remove(cur);
        return true;
    }
}
```

# Subarray (preSum)

## [523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

Given an integer array nums and an integer k, return true if nums has a continuous subarray of size at least two whose elements sum up to a multiple of k, or false otherwise.

An integer x is a multiple of k if there exists an integer n such that x = n * k. 0 is always a multiple of k.

**Example 1:**

```
Input: nums = [23,2,4,6,7], k = 6
Output: true
Explanation: [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.
```

**Example 2:**

```
Input: nums = [23,2,6,4,7], k = 6
Output: true
Explanation: [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.
```

**Example 3:**

```
Input: nums = [23,2,6,4,7], k = 13
Output: false
```

思路分析：

- 使用 HashMap 记录前缀和取模的值，以及该值对应的索引。`Map<Integer, Integer> map = new HashMap<>()`; 键为`preSum % k`, 值为索引。
- 在计算前缀和的过程中，如果前缀和取模的值出现重复，两个重复值之间的间隔大于 1（题目要求子数组长度至少为 2）则返回 true，否则循环结束后返回 false。然后这里我们来详细证明一下为什么这样做是正确的。
- 首先，得知道`(A + B) % k = (A % k + B) % k`，加法减法没有区别，也就是`(A - B) % k = (A % k - B) % k`。
  - 使用 sum 表示前缀和根据题目要求，`(sum[j] - sum[i]) % k = 0`也就是`(sum[j] % k - sum[i]) % k = 0`也就是`sum[j] % k - sum[i] = n * k`，其中 n 为非负整数。
  - 然后对上式两边对 k 取余，即`sum[j] % k % k - sum[i] % k = n * k % k`，显然左边第一项就等于`sun[j] % k`，右边为 0。
  - 移项就得到`sum[j] % k = sum[i] % k`。
  - 也就是说，如果子数组`[i, j - 1]`的和`sum[j] - sum[i]`是 k 的 n 倍，那么`sum[j] % k = sum[i] % k`。
- 所以通过哈希表去查看是否会在求前缀和的过程中对 k 取模得到的值相等即可，也就是`map.containsKey(temp)`，记录每一个取余结果的索引的意义是：要求子数组的长度至少为 2，所以当出现对 k 取模相等时，还要判断`i - map.get(temp) > 1`，如果成立，就返回 true; 否则继续循环。
  - 注意这里不需要更新索引，因为是要求数组的最小长度，所以记录第一个索引就好。
- 首先要将表示没有元素的那种情况记录`map.put(0, -1)`;
- 其次要特殊处理 k 为 0 的情况，此时不能取模，这种情况是要求前缀和之差为 0 也就是`sum[i] == sum[j]`，所以 map 记录的就是前缀和而不是取余。所以在累加`sum += nums[i]`; 之后记录值`map.put(temp, i)`; 之前，要先处理在 map 中记录什么值`int temp = k == 0 ? sum : sum % k`。

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {

    public boolean checkSubarraySum(int[] nums, int k) {
        int sum = 0;

        // key：区间 [0..i] 里所有元素的和 % k
        // value：下标 i
        Map<Integer, Integer> map = new HashMap<>();
        // 理解初始化的意义
        map.put(0, -1);
        int len = nums.length;
        for (int i = 0; i < len; i++) {
            sum += nums[i];
            if (k != 0) {
                sum = sum % k;
            }
  
            if (map.containsKey(sum)) {
                if (i - map.get(sum) > 1) {
                    return true;
                }
            } else {
                map.put(sum, i);
            }

        }
        return false;
    }
}
```

# DFS

## Tree Question

### [1469. Find All The Lonely Nodes](https://leetcode.com/problems/find-all-the-lonely-nodes/)

In a binary tree, a lonely node is a node that is the only child of its parent node. The root of the tree is not lonely because it does not have a parent node.

Given the root of a binary tree, return an array containing the values of all lonely nodes in the tree. Return the list in any order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/06/03/e1.png)

Input: root = [1,2,3,null,4]
Output: [4]
Explanation: Light blue node is the only lonely node.
Node 1 is the root and is not lonely.
Nodes 2 and 3 have the same parent and are not lonely.
这个题的思路就是利用 dfs 去查找孤独的子节点，为了方便 dfs，我们不仅需要传入当前的 node，还需要把 node 的 parent 也传进去，方便我们做判断。

要注意的是，虽然传入的是两个 node，但是跳出递归的条件还是 `node==null`

```java
class Solution {
    List<Integer> res = new ArrayList<>();
    public List<Integer> getLonelyNodes(TreeNode root) {
        dfs(root, null);
        return res;
  
    }
    private void dfs(TreeNode node, TreeNode parent){
        // 递归跳出的条件
        if(node == null){
            return;
        }
        // 理论上来说如果可以执行到这里，那么 parent 必不可能是 null
        // 那么为什么要带 parent != null 这个判断条件呢？
        // 看起来多此一举，其实是为了递归第一次开始的条件特判，也可以不写，那这样的话，递归开始就要写成：
        // dfs(root.left, root);
        // dfs(root.right, root);
        // 一样的道理
        if(parent != null && (parent.left == null || parent.right == null)){
            res.add(node.val);
        }
        dfs(node.left, node);
        dfs(node.right, node);
    }
}
```

### [897. Increasing Order Search Tree](https://leetcode.com/problems/increasing-order-search-tree/)

Given the root of a binary search tree, rearrange the tree in in-order so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg)

Input: root = [5,3,6,2,4,null,8,1,null,null,null,7,9]
Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
这个题算是比较简单的 dfs，思路很好想，首先这是个 BST, 直接 inorder 然后重新构建一棵树就好。中间用 List 临时保存节点。
需要注意一点是，最好保存 node.val，构建新树的时候，初始化新节点，这样做的好处就是不用担心之前的树结构，否则我们需要在构建节点前先清空之前的节点结构。

```java
class Solution {
    List<TreeNode> res = new ArrayList<>();
    public TreeNode increasingBST(TreeNode root) {
        inorder(root);
        TreeNode tmp = new TreeNode();
        TreeNode cur = tmp;
        for(TreeNode node : res){
            // 重点就在这里，我们构建新节点之前，需要先清空原来节点的结构，防止产生环
            // 当然更好的操作是如下：
            // cur.right = new TreeNode(node.val);
            node.left = null;
            node.right = null;
            cur.left = null;
            cur.right = node;
            cur = cur.right;
  
        }
        return tmp.right;
    }
    private void inorder(TreeNode node){
        if(node == null){
            return;
        }
        inorder(node.left);
        res.add(node);
        inorder(node.right);
    }
}
```

### [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any path.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

```java
class Solution {
    int res = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return res;
    }
    private int dfs(TreeNode node){
        if(node == null){
            return 0;
        }
  
        int left = Math.max(dfs(node.left), 0);
        int right = Math.max(dfs(node.right), 0);
        int tmp = left + right + node.val;
        res = Math.max(res, tmp);
        return node.val + Math.max(left, right);
    }
}
```

### [1448. Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)

Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

**Example 1:**
![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)

Input: root = [3,1,4,3,null,1,5]
Output: 4
Explanation: Nodes in blue are good.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.
dfs解题，要注意一点，搜索的过程中保存当前最大节点值即可

```java
class Solution {
    int res = 0;
    public int goodNodes(TreeNode root) {
        dfs(root, Integer.MIN_VALUE);
        return res;
  
    }
    private void dfs(TreeNode node, int max){
        if(node == null){
            return;
        }
        if(node.val >= max){
            res++;
            max = node.val;
        }
        dfs(node.left, max);
        dfs(node.right, max);
    }
}
```

## Island Question

### 网格类问题的 DFS 遍历方法

#### 网格问题的基本概念

我们首先明确一下岛屿问题中的网格结构是如何定义的，以方便我们后面的讨论。

网格问题是由 m×n 个小方格组成一个网格，每个小方格与其上下左右四个方格认为是相邻的，要在这样的网格上进行某种搜索。

岛屿问题是一类典型的网格问题。每个格子中的数字可能是 0 或者 1。我们把数字为 0 的格子看成海洋格子，数字为 1 的格子看成陆地格子，这样相邻的陆地格子就连接成一个岛屿。

![](https://pic.leetcode-cn.com/c36f9ee4aa60007f02ff4298bc355fd6160aa2b0d628c3607c9281ce864b75a2.jpg)

在这样一个设定下，就出现了各种岛屿问题的变种，包括岛屿的数量、面积、周长等。不过这些问题，基本都可以用 DFS 遍历来解决。

#### DFS 的基本结构

网格结构要比二叉树结构稍微复杂一些，它其实是一种简化版的**图**结构。要写好网格上的 DFS 遍历，我们首先要理解二叉树上的 DFS 遍历方法，再类比写出网格结构上的 DFS 遍历。我们写的二叉树 DFS 遍历一般是这样的：

```java
void traverse(TreeNode root) {
    // 判断 base case
    if (root == null) {
        return;
    }
    // 访问两个相邻结点：左子结点、右子结点
    traverse(root.left);
    traverse(root.right);
}
```

可以看到，二叉树的 DFS 有两个要素：**「访问相邻结点」** 和 **「判断 base case」**。

第一个要素是**访问相邻结点**。二叉树的相邻结点非常简单，只有左子结点和右子结点两个。二叉树本身就是一个递归定义的结构：一棵二叉树，它的左子树和右子树也是一棵二叉树。那么我们的 DFS 遍历只需要递归调用左子树和右子树即可。

第二个要素是 **判断 base case**。一般来说，二叉树遍历的 base case 是 `root == null`。这样一个条件判断其实有两个含义：一方面，这表示 root 指向的子树为空，不需要再往下遍历了。另一方面，在 `root == null` 的时候及时返回，可以让后面的 `root.left` 和 `root.right` 操作不会出现空指针异常。

对于网格上的 DFS，我们完全可以参考二叉树的 DFS，写出网格 DFS 的两个要素：

首先，网格结构中的格子有多少相邻结点？答案是上下左右四个。对于格子 (r, c) 来说（r 和 c 分别代表行坐标和列坐标），四个相邻的格子分别是 (r-1, c)、(r+1, c)、(r, c-1)、(r, c+1)。换句话说，网格结构是「四叉」的。

![](https://pic.leetcode-cn.com/63f5803e9452ccecf92fa64f54c887ed0e4e4c3434b9fb246bf2b410e4424555.jpg)

其次，网格 DFS 中的 base case 是什么？从二叉树的 base case 对应过来，应该是网格中不需要继续遍历、grid[r][c] 会出现数组下标越界异常的格子，也就是那些超出网格范围的格子。

![](https://pic.leetcode-cn.com/5a91ec351bcbe8e631e7e3e44e062794d6e53af95f6a5c778de369365b9d994e.jpg)

这一点稍微有些反直觉，坐标竟然可以临时超出网格的范围？这种方法我称为「先污染后治理」—— 甭管当前是在哪个格子，先往四个方向走一步再说，如果发现走出了网格范围再赶紧返回。这跟二叉树的遍历方法是一样的，先递归调用，发现 `root == null` 再返回。

这样，我们得到了网格 DFS 遍历的框架代码：

```java
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    // 如果坐标 (r, c) 超出了网格范围，直接返回
    if (!inArea(grid, r, c)) {
        return;
    }
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

#### 如何避免重复遍历

网格结构的 DFS 与二叉树的 DFS 最大的不同之处在于，遍历中可能遇到遍历过的结点。这是因为，网格结构本质上是一个「图」，我们可以把每个格子看成图中的结点，每个结点有向上下左右的四条边。在图中遍历时，自然可能遇到重复遍历结点。

这时候，DFS 可能会不停地「兜圈子」，永远停不下来，如下图所示：

![](https://pic.leetcode-cn.com/7fec64afe8ab72c5df17d6a41a9cc9ba3879f58beec54a8791cbf108b9fd0685.gif)

如何避免这样的重复遍历呢？答案是标记已经遍历过的格子。以岛屿问题为例，我们需要在所有值为 1 的陆地格子上做 DFS 遍历。每走过一个陆地格子，就把格子的值改为 2，这样当我们遇到 2 的时候，就知道这是遍历过的格子了。也就是说，每个格子可能取三个值：

- 0 —— 海洋格子
- 1 —— 陆地格子（未遍历过）
- 2 —— 陆地格子（已遍历过）

我们在框架代码中加入避免重复遍历的语句：

```java
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    if (!inArea(grid, r, c)) {
        return;
    }
    // 如果这个格子不是岛屿，直接返回
    if (grid[r][c] != 1) {
        return;
    }
    grid[r][c] = 2; // 将格子标记为「已遍历过」
  
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

![](https://pic.leetcode-cn.com/20fe202fb5e5fc5048e140c29310c1bcbb17661860d2441e8a3feb1236a2e44d.gif)

这样，我们就得到了一个岛屿问题、乃至各种网格问题的通用 DFS 遍历方法。以下所讲的几个例题，其实都只需要在 DFS 遍历框架上稍加修改而已。

```
小贴士：

在一些题解中，可能会把「已遍历过的陆地格子」标记为和海洋格子一样的 0，美其名曰「陆地沉没方法」，即遍历完一个陆地格子就让陆地「沉没」为海洋。这种方法看似很巧妙，但实际上有很大隐患，因为这样我们就无法区分「海洋格子」和「已遍历过的陆地格子」了。如果题目更复杂一点，这很容易出 bug。
```

### [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input: grid = [
["1","1","1","1","0"],
["1","1","0","1","0"],
["1","1","0","0","0"],
["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**

```
Input: grid = [
["1","1","0","0","0"],
["0","0","1","0","0"],
["0","0","0","1","1"]
]
Output: 3
```

```java
class Solution {
    int m; 
    int n;
    public int numIslands(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        int res = 0;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == '1'){
                    dfs(grid, i, j);
                    res++;
                }
            }
        }
        return res;
    }
  
    private void dfs(char[][] grid, int x, int y){
        if(x < 0 || x > m - 1|| y < 0 || y > n - 1 || grid[x][y] != '1'){
            return;
        }
        grid[x][y] = '0';
        dfs(grid, x + 1, y);
        dfs(grid, x - 1, y);
        dfs(grid, x, y + 1);
        dfs(grid, x, y - 1);
    }
}
```

### [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/)

Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

**Example 1:**

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
[0,0,0,0,0,0,0,1,1,1,0,0,0],
[0,1,1,0,1,0,0,0,0,0,0,0,0],
[0,1,0,0,1,1,0,0,1,0,1,0,0],
[0,1,0,0,1,1,0,0,1,1,1,0,0],
[0,0,0,0,0,0,0,0,0,0,1,0,0],
[0,0,0,0,0,0,0,1,1,1,0,0,0],
[0,0,0,0,0,0,0,1,1,0,0,0,0]]
Given the above grid, return 6. Note the answer is not 11, because the island must be connected 4-directionally.
```

**Example 2:**

```
[[0,0,0,0,0,0,0,0]]
```

Given the above grid, return 0.

**Note**: The length of each dimension in the given grid does not exceed 50.

```
给定一个包含了一些 0 和 1 的非空二维数组 grid，一个岛屿是一组相邻的 1（代表陆地），这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表海洋）包围着。

找到给定的二维数组中最大的岛屿面积。如果没有岛屿，则返回面积为 0 。
```

这道题目只需要对每个岛屿做 DFS 遍历，求出每个岛屿的面积就可以了。求岛屿面积的方法也很简单，代码如下，每遍历到一个格子，就把面积加一。

```java
int area(int[][] grid, int r, int c) {  
    return 1 
        + area(grid, r - 1, c)
        + area(grid, r + 1, c)
        + area(grid, r, c - 1)
        + area(grid, r, c + 1);
}
```

最终我们得到的完整题解代码如下：

```java
public int maxAreaOfIsland(int[][] grid) {
    int res = 0;
    for (int r = 0; r < grid.length; r++) {
        for (int c = 0; c < grid[0].length; c++) {
            if (grid[r][c] == 1) {
                int a = area(grid, r, c);
                res = Math.max(res, a);
            }
        }
    }
    return res;
}

int area(int[][] grid, int r, int c) {
    if (!inArea(grid, r, c)) {
        return 0;
    }
    if (grid[r][c] != 1) {
        return 0;
    }
    grid[r][c] = 2;
  
    return 1 
        + area(grid, r - 1, c)
        + area(grid, r + 1, c)
        + area(grid, r, c - 1)
        + area(grid, r, c + 1);
}

boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

### [463. Island Perimeter](https://leetcode.com/problems/island-perimeter/)

You are given row x col grid representing a map where grid[i][j] = 1 represents land and grid[i][j] = 0 represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/island.png)

```
Input: grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
Output: 16
Explanation: The perimeter is the 16 yellow stripes in the image above.
```

**Example 2:**

```
Input: grid = [[1]]
Output: 4
```

**Example 3:**

```
Input: grid = [[1,0]]
Output: 4
```

实话说，这道题用 DFS 来解并不是最优的方法。对于岛屿，直接用数学的方法求周长会更容易。不过这道题是一个很好的理解 DFS 遍历过程的例题，不信你跟着我往下看。

我们再回顾一下 网格 DFS 遍历的基本框架：

```java
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    if (!inArea(grid, r, c)) {
        return;
    }
    // 如果这个格子不是岛屿，直接返回
    if (grid[r][c] != 1) {
        return;
    }
    grid[r][c] = 2; // 将格子标记为「已遍历过」
  
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

可以看到，dfs 函数直接返回有这几种情况：

- !inArea(grid, r, c)，即坐标 (r, c) 超出了网格的范围，也就是我所说的「先污染后治理」的情况
- grid[r][c] != 1，即当前格子不是岛屿格子，这又分为两种情况：
  - grid[r][c] == 0，当前格子是海洋格子
  - grid[r][c] == 2，当前格子是已遍历的陆地格子
    那么这些和我们岛屿的周长有什么关系呢？实际上，岛屿的周长是计算岛屿全部的「边缘」，而这些边缘就是我们在 DFS 遍历中，dfs 函数返回的位置。观察题目示例，我们可以将岛屿的周长中的边分为两类，如下图所示。黄色的边是与网格边界相邻的周长，而蓝色的边是与海洋格子相邻的周长。

![](https://pic.leetcode-cn.com/66d817362c1037ebe7705aacfbc6546e321c2b6a2e4fec96791f47604f546638.jpg)

当我们的 dfs 函数因为「坐标 (r, c) 超出网格范围」返回的时候，实际上就经过了一条黄色的边；而当函数因为「当前格子是海洋格子」返回的时候，实际上就经过了一条蓝色的边。这样，我们就把岛屿的周长跟 DFS 遍历联系起来了，我们的题解代码也呼之欲出：

```java
public int islandPerimeter(int[][] grid) {
    for (int r = 0; r < grid.length; r++) {
        for (int c = 0; c < grid[0].length; c++) {
            if (grid[r][c] == 1) {
                // 题目限制只有一个岛屿，计算一个即可
                return dfs(grid, r, c);
            }
        }
    }
    return 0;
}

int dfs(int[][] grid, int r, int c) {
    // 函数因为「坐标 (r, c) 超出网格范围」返回，对应一条黄色的边
    if (!inArea(grid, r, c)) {
        return 1;
    }
    // 函数因为「当前格子是海洋格子」返回，对应一条蓝色的边
    if (grid[r][c] == 0) {
        return 1;
    }
    // 函数因为「当前格子是已遍历的陆地格子」返回，和周长没关系
    if (grid[r][c] != 1) {
        return 0;
    }
    grid[r][c] = 2;
    return dfs(grid, r - 1, c)
        + dfs(grid, r + 1, c)
        + dfs(grid, r, c - 1)
        + dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

### [130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

Given an `m x n` matrix `board` containing `'X'` and `'O'`, capture all regions surrounded by 'X'.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
Explanation: Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.
针对这个题，第一思路肯定是dfs，但是有一点点脑筋急转弯，如果按照正常的搜索思路来的话，我们怎么区别当前岛屿跟边界接壤呢？如果说我们的搜索搜索到边界了，那么我们怎么给之前的路线打上标记呢?

那么问题来了，我们可以逆向思维，我们从边界的岛屿开始搜索，如果说存在完全独立的岛屿，那么我们是肯定不会搜索到的。所以我们从四条边开始，对于这些岛屿我们打上标记。搜索完成后我们再次进行遍历，将打上标签的岛屿恢复为 `'O'`，将为 `'O'`的岛屿标记为 `'X'`即可。

```java
class Solution {
    int m;
    int n;
    int[][] dir = {{1, 0},{-1, 0},{0 ,1},{0, -1}};
    public void solve(char[][] board) {
        m = board.length;
        n = board[0].length;
        for(int i = 0; i < m; i++){
            dfs(board, i, 0);
            dfs(board, i, n - 1);
        }
        for(int i = 0; i < n; i++){
            dfs(board, 0, i);
            dfs(board, m - 1, i);
        }
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(board[i][j] == 'F'){
                    board[i][j] = 'O';
                }else if(board[i][j] == 'O'){
                    board[i][j] = 'X';
                }
            }
        }
  
    }
    private void dfs(char[][] board, int x, int y){
        if(x < 0 || x > m - 1 || y < 0 || y > n - 1){
            return;
        }
        if(board[x][y] != 'O'){
            return;
        }
        board[x][y] = 'F';
        for(int[] d : dir){
            int nx = x + d[0];
            int ny = y + d[1];
            dfs(board, nx ,ny);
        }
  
    }
}
```

## [1631. Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

You are a hiker preparing for an upcoming hike. You are given heights, a 2D array of size rows x columns, where heights[row][col] represents the height of cell (row, col). You are situated in the top-left cell, (0, 0), and you hope to travel to the bottom-right cell, (rows-1, columns-1) (i.e., 0-indexed). You can move up, down, left, or right, and you wish to find a route that requires the minimum effort.

A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.

Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)

Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2
Explanation: The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.
**Constraints:**

- rows == heights.length
- columns == heights[i].length
- 1 <= rows, columns <= 100
- 1 <= heights[i][j] <= 1000000

这个题如果没有这些条件限制，其实是一道不难的 dfs 题。问题在于我们要找出 Mini Effort。

所以一开始我并没有太多的思考用 dfs 和回溯，结果肯定是超时的。后来看到解析中说可以用二分法来缩短时间，觉得这个思路很好。

二分法其实是转换了思路，因为题目给出了 `1 <= heights[i][j] <= 1000000`，所以最大的 effort 也不会超过 1000000，所以我们可以在 0 和 1000000 这个范围中不断地二分搜索，去看每一步的 effort 在不在这个范围内，如果在，缩小二分边界，如果不在，证明找到了最小值。

```java
class Solution {
    int m;
    int n;
    int[][] dir = {{0, 1},{0, -1},{1, 0},{-1, 0}};
    public int minimumEffortPath(int[][] heights) {
        m = heights.length;
        n = heights[0].length;
        // 二分搜索的起始左右边界
        int left = 0;
        int right = 1000000;
        // 普通二分搜索套路
        while(left < right){
            int mid = (left + right) / 2;
            // 判断条件是当前 mid left right 条件下， dfs 能否搜索到
            // 注意，这里有个小坑，一开始我在 loop 外初始化 vis 数组，然后传入 dfs，结果报错
            // 原因就是每一次二分搜索都是一次独立的 dfs 全过程，所以我们的 vis 数组应该在每一次二分搜索时候重置
            if(dfs(heights, 0, 0, mid, new boolean[m][n])){
                right = mid;
            }else{
                left = mid + 1;
            }
        }
        return left;
    }
    private boolean dfs(int[][] heights, int x, int y, int mid, boolean[][] vis){
        // 判断条件：当走到图的右下角的时候，证明存在这样一条路径，return true
        if(x == m - 1 && y == n - 1){
            return true;
        }
        // 修改 vis 状态
        vis[x][y] = true;
        for(int[] d : dir){
            int nx = x + d[0];
            int ny = y + d[1];
            // 递归条件：不出界，未访问，高度差满足要求
            if(nx >= 0 && nx < m && ny >= 0 && ny < n && !vis[nx][ny] && Math.abs(heights[nx][ny] - heights[x][y]) <= mid){
                if(dfs(heights, nx, ny, mid, vis)){
                    return true;
                }
            }
        }
        return false;
    }
}
```

# BFS

**什么情况应当用 BFS 搜索**

我们都知道 DFS（深度优先搜索）和 BFS（广度优先搜索）。它们各有不同的适应场景。

![](https://pic.leetcode-cn.com/725e473003c35e3be67ac6177cc6744fa04b0466795b5e69c7d673f626206b86-file_1583293748397)

BFS 可以看成是层序遍历。从某个结点出发，BFS 首先遍历到距离为 1 的结点，然后是距离为 2、3、4…… 的结点。因此，BFS 可以用来**求最短路径问题**。BFS 先搜索到的结点，一定是距离最近的结点。

再看看这道题的题目要求：返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。翻译一下，实际上就是求**腐烂橘子到所有新鲜橘子的最短路径**。那么这道题使用 BFS，应该是毫无疑问的了。

**如何写（最短路径的） BFS 代码**

我们都知道 BFS 需要使用队列，代码框架是这样子的（伪代码）：

```python
while queue 非空：
	node = queue.pop()
    for node 的所有相邻结点 m:
        if m 未访问过：
            queue.push(m)
```

但是用 BFS 来求最短路径的话，这个队列中第 1 层和第 2 层的结点会紧挨在一起，无法区分。因此，我们需要稍微修改一下代码，在每一层遍历开始前，记录队列中的结点数量 nn ，然后一口气处理完这一层的 nn 个结点。代码框架是这样的：

```python
depth = 0 # 记录遍历到第几层
while queue 非空：
    depth++
    n = queue 中的元素个数
    循环 n 次：
        node = queue.pop()
        for node 的所有相邻结点 m:
            if m 未访问过：
                queue.push(m)
```

## 994 Rotting Oranges

In a given grid, each cell can have one of three values:

- the value 0 representing an empty cell;
- the value 1 representing a fresh orange;
- the value 2 representing a rotten orange.

Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.

**Example 1:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200830000451.png)

```
Input: [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

**Example 2:**

```
Input: [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation:  The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
```

**Example 3:**

```
Input: [[0,2]]
Output: 0
Explanation:  Since there are already no fresh oranges at minute 0, the answer is just 0.
```

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

## [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

- For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
  Return true if you can finish all courses. Otherwise, return false.

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

这道题是典型的用拓扑排序的题目，我们考虑拓扑排序中最前面的节点，该节点一定不会有任何入边，也就是它没有任何的先修课程要求。当我们将一个节点加入答案中后，我们就可以移除它的所有出边，代表着它的相邻节点少了一门先修课程的要求。如果某个相邻节点变成了「没有任何入边的节点」，那么就代表着这门课可以开始学习了。按照这样的流程，我们不断地将没有入边的节点加入答案，直到答案中包含所有的节点（得到了一种拓扑排序）或者不存在没有入边的节点（图中包含环）。

上面的想法类似于广度优先搜索，因此我们可以将广度优先搜索的流程与拓扑排序的求解联系起来。

```java
class Solution {
    List<List<Integer>> edges;
    int[] indeg;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // 邻接表：通过结点的索引，我们能够得到这个结点的后继结点
        edges = new ArrayList<List<Integer>>();
        for (int i = 0; i < numCourses; ++i) {
            edges.add(new ArrayList<Integer>());
        }
        // 入度数组：通过结点的索引，我们能够得到指向这个结点的结点个数
        indeg = new int[numCourses];
        for (int[] info : prerequisites) {
            edges.get(info[1]).add(info[0]);
            ++indeg[info[0]];
        }

        Queue<Integer> queue = new LinkedList<Integer>();
        // 扫描入度数组，将入度为 0 的放入队列中
        for (int i = 0; i < numCourses; ++i) {
            if (indeg[i] == 0) {
                queue.offer(i);
            }
        }

        // 记录已经出队的课程数量
        int visited = 0;
        // 只要队列非空，就从队首取出入度为 0 的节点
        while (!queue.isEmpty()) {
            ++visited;
            int u = queue.poll();
            // 遍历当前出队节点的所有后继节点
            for (int v: edges.get(u)) {
                --indeg[v];
                if (indeg[v] == 0) {
                    queue.offer(v);
                }
            }
        }

        return visited == numCourses;
    }
}
```

## [752. Open the Lock](https://leetcode.com/problems/open-the-lock/)

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example we can turn `'9'` to be `'0'`, or `'0'` to be `'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at `'0000'`, a string representing the state of the 4 wheels.

You are given a list of deadends dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a target representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or `-1` if it is impossible.

**Example 1:**

Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
这个题的本质其实是在求最短路径，转化成图的问题就是 BFS 求最短路径。

由于是转轮锁，所以我们一次有两个选择，往上拨或者往下拨，所以四位数的密码一次就有八种可能，这就把这个问题转换成了 8 叉的 dfs。

只需要注意这三个问题：

1. 密码的临界问题，9 往前就是 0，0 往后就是 9。
2. 走回头路。比如说我们从 "0000" 拨到 "1000"，但是等从队列拿出 "1000" 时，还会拨出一个 "0000"，这样的话会产生死循环。
3. 终止条件，按照题目要求，我们找到 target 就应该结束并返回拨动的次数。
4. 对 deadends 的处理，按道理这些「死亡密码」是不能出现的，也就是说你遇到这些密码的时候需要跳过。

```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> dead = new HashSet<>();
        for(String s : deadends){
            dead.add(s);
        }
        Set<String> vis = new HashSet<>();
        Queue<String> q = new LinkedList<>();
        int res = 0;
        q.offer("0000");
        vis.add("0000");
  
        while(!q.isEmpty()){
            int size = q.size();
            for(int i = 0; i < size; i++){
                String tmp = q.poll();
                if(dead.contains(tmp)){
                    continue;
                }
                if(tmp.equals(target)){
                    return res;
                }
                for(int j = 0; j < 4; j++){
                    String up = up(tmp, j);
                    if(!vis.contains(up)){
                        q.offer(up);
                        vis.add(up);
                    }
                    String down = down(tmp, j);
                    if(!vis.contains(down)){
                        q.offer(down);
                        vis.add(down);
                    }
                }
            }
            res++;

        }
        return -1;
    }
    private String up(String s, int index){
        char[] c = s.toCharArray();
        if(c[index] == '9'){
            c[index] = '0';
        }else{
            c[index]++;
        }
        return String.valueOf(c);
    }
    private String down(String s, int index){
        char[] c = s.toCharArray();
        if(c[index] == '0'){
            c[index] = '9';
        }else{
            c[index]--;
        }
        return String.valueOf(c);
    }
}
```
# Binary Search
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726184651.png)

# Divide and Conquer (分治法)

**Divide and conquer concept**
1. Divide the problem into a number of subproblems that are smaller instances of the same problem.
2. Conquer the subproblems by solving them recursively. If the subproblem sizes are small enough, however, just solve the subproblems in a straightforward manner.
3. Combine the solutions to the subproblems into the solution for the original problem.

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726185406.png)


## [Closest Pair of Points using Divide and Conquer algorithm](https://www.geeksforgeeks.org/closest-pair-of-points-using-divide-and-conquer-algorithm/)

We are given an array of n points in the plane, and the problem is to find out the closest pair of points in the array. This problem arises in a number of applications. For example, in air-traffic control, you may want to monitor planes that come too close together, since this may indicate a possible collision. Recall the following formula for distance between two points p and q.
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210728113647.png)   
The Brute force solution is O(n^2), compute the distance between each pair and return the smallest. We can calculate the smallest distance in O(nLogn) time using Divide and Conquer strategy. In this post, a O(n x (Logn)^2) approach is discussed. We will be discussing a O(nLogn) approach in a separate post.

**Algorithm**
Following are the detailed steps of a O(n (Logn)^2) algorithm. 
Input: An array of n points P[] 
Output: The smallest distance between two points in the given array.

1. Find the middle point in the sorted array, we can take `P[n/2]` as middle point. 
2. Divide the given array in two halves. The first subarray contains points from `P[0]` to `P[n/2]`. The second subarray contains points from `P[n/2+1]` to `P[n-1]`.
3. Recursively find the smallest distances in both subarrays. Let the distances be dl and dr. Find the minimum of dl and dr. Let the minimum be d.
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210728113854.png)
4. From the above 3 steps, we have an upper bound d of minimum distance. Now we need to consider the pairs such that one point in pair is from the left half and the other is from the right half. Consider the vertical line passing through `P[n/2]` and find all points whose x coordinate is closer than d to the middle vertical line. Build an array strip[] of all such points. 
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210728113910.png)
5. Sort the array strip[] according to y coordinates. This step is O(nLogn). It can be optimized to O(n) by recursively sorting and merging. 
6. Find the smallest distance in strip[]. This is tricky. From the first look, it seems to be a O(n^2) step, but it is actually O(n). It can be proved geometrically that for every point in the strip, we only need to check at most 7 points after it (note that strip is sorted according to Y coordinate). See this for more analysis.
7. Finally return the minimum of d and distance calculated in the above step (step 6)  

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class ClosestPairOfPoints {

    private long findClosedPair(int[] xs, int[] ys){
        int len = xs.length;
        Point[] points = new Point[len];
        for (int i = 0; i < len; i++) {
            points[i] = new Point(xs[i], ys[i]);
        }
        return divide(0, len - 1, points);
    }

    private long divide(int left, int right, Point[] points){
        
        long curMinDis = Long.MAX_VALUE; // 当前最小两点距离，初始值设置为无穷大
        if(left == right) return curMinDis; // 如果只有一个点，则不存在最近两点距离，返回无穷大
        if(left + 1 == right) return calDistance(points[left], points[right]); // 这里是判断是否只有两个点，如果只有两个点的话，直接求解
        
        // 分治法：第一步：分区，并求取左右分区最小两点距离
        int mid = left + (right - left) / 2;
        long leftMinDis = divide(left, mid, points); // 通过右移运算除2，对区域进行合理的划分，使得左右两边保持大致相等个数点
        long rightMinDis = divide(mid, right, points);
        curMinDis = Math.min(leftMinDis, rightMinDis);

        // 分治法：第二步：假设距离最近的两点分别在左右分区中
        // 关键代码，距离最近的两个点，一个位于左边区域，一个位于右边区域，x轴搜索范围[mid - cueMinDis, mid + curMinDis]
        // 记录搜索区间内的点的索引，便于进一步计算最小距离
        List<Integer> validPointIndex = new ArrayList<>();
        for (int i = left; i <= right; i++) {
            if(Math.abs(points[mid].x - points[i].x)<= curMinDis){
                validPointIndex.add(i);
            }
        }
        Collections.sort(validPointIndex, (a, b) -> points[a].y - points[b].y);
        for (int i = 0; i < validPointIndex.size() - 1; i++) { // 基于索引，进一步计算区间内最小两点距离
            for (int j = i + 1; j < validPointIndex.size(); j++) {
                // 如果区间内的两点y轴距大于curMinDis，则没有必要计算了，因为，他们的距离肯定大于curMinDis
                if(Math.abs(points[validPointIndex.get(i)].y - points[validPointIndex.get(j)].y) > curMinDis){
                    continue;
                }
                long tmpDis = calDistance(points[validPointIndex.get(i)], points[validPointIndex.get(j)]);
                curMinDis = Math.min(curMinDis, tmpDis);
            }
        }
        return curMinDis;
    }

    private long calDistance(Point p1, Point p2){
        return (long) Math.sqrt((p2.y - p1.y) * (p2.y - p1.y) + (p2.x - p1.x) * (p2.x - p1.x));
    }

    public static void main(String[] args) {
        int[] xs = {2, 12, 40, 5, 12, 3};
        int[] ys = {3, 30, 50, 1, 10, 4};
        ClosestPairOfPoints cpp = new ClosestPairOfPoints();
        long res = cpp.findClosedPair(xs, ys);
        System.out.println(res);
    }
}
// 自定义Point类去封装所有的点，便于计算
class Point{
    int x;
    int y;
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

## [Count Inversions in an array](https://www.geeksforgeeks.org/counting-inversions/)

Inversion Count for an array indicates – how far (or close) the array is from being sorted. If the array is already sorted, then the inversion count is 0, but if the array is sorted in the reverse order, the inversion count is the maximum. 
Formally speaking, two elements `a[i]` and `a[j]` form an inversion if `a[i]` > `a[j]` and i < j 

**Example:**

    Input: arr[] = {8, 4, 2, 1}
    Output: 6

    Explanation: Given array has six inversions:
    (8, 4), (4, 2), (8, 2), (8, 1), (4, 1), (2, 1).


    Input: arr[] = {3, 1, 2}
    Output: 2

    Explanation: Given array has two inversions:
    (3, 1), (3, 2) 

**Algorithm:**
1. The idea is similar to merge sort, divide the array into two equal or almost equal halves in each step until the base case is reached.
2. Create a function merge that counts the number of inversions when two halves of the array are merged, create two indices `i` and `j`, i is the index for the first half, and j is an index of the second half. if `a[i]` is greater than `a[j]`, then there are `(mid – i)` inversions. because left and right subarrays are sorted, so all the remaining elements in left-subarray `(a[i+1], a[i+2] … a[mid])` will be greater than `a[j]`.
3. Create a recursive function to divide the array into halves and find the answer by summing the number of inversions is the first half, the number of inversion in the second half and the number of inversions by merging the two.
4. The base case of recursion is when there is only one element in the given half.
5. Print the answer


**highly recommond this video!**
<div style="position: relative; padding: 30% 45%;">
<iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src="https://player.bilibili.com/player.html?aid=52676501&bvid=BV114411J7QQ&cid=92185430&page=1&as_wide=1&high_quality=1&danmaku=0" frameborder="no" scrolling="no" allowfullscreen="true"></iframe>
</div>


```java
// Java implementation of the approach
import java.util.Arrays;

public class GFG {

	// Function to count the number of inversions
	// during the merge process
	private static int mergeAndCount(int[] arr, int l,
									int m, int r)
	{

		// Left subarray
		int[] left = Arrays.copyOfRange(arr, l, m + 1);

		// Right subarray
		int[] right = Arrays.copyOfRange(arr, m + 1, r + 1);

		int i = 0, j = 0, k = l, swaps = 0;

		while (i < left.length && j < right.length) {
			if (left[i] <= right[j])
				arr[k++] = left[i++];
			else {
				arr[k++] = right[j++];
				swaps += (m + 1) - (l + i);
			}
		}
		while (i < left.length)
			arr[k++] = left[i++];
		while (j < right.length)
			arr[k++] = right[j++];
		return swaps;
	}

	// Merge sort function
	private static int mergeSortAndCount(int[] arr, int l,
										int r)
	{

		// Keeps track of the inversion count at a
		// particular node of the recursion tree
		int count = 0;

		if (l < r) {
			int m = (l + r) / 2;

			// Total inversion count = left subarray count
			// + right subarray count + merge count

			// Left subarray count
			count += mergeSortAndCount(arr, l, m);

			// Right subarray count
			count += mergeSortAndCount(arr, m + 1, r);

			// Merge count
			count += mergeAndCount(arr, l, m, r);
		}

		return count;
	}

	// Driver code
	public static void main(String[] args)
	{
		int[] arr = { 1, 20, 6, 4, 5 };

		System.out.println(
			mergeSortAndCount(arr, 0, arr.length - 1));
	}
}

// This code is contributed by Pradip Basak


```
# Topological Sorting

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210623104044.png)

BFS入度法（Kahn算法）

1. 建图
2. 建入度
3. 找入口
4. BFS拓扑排序

```java
public boolean canFinish(int N, int[][] edges){
    Map<Integer, List<Integer>> graph = new HashMap<>();
    int[] indegree = new int[N];
    for(int[] edge : edges){
        int end = edge[0], start = edge[1];
        graph.computeIfAbsent(start, x -> new ArrayList<>()).add(end);
        indegree[end]++;
    }
    Deque<Integer> deque = new ArrayDeque<>();
    for(int i = 0; i < N; i++){
        if(indegree[i] == 0){
            deque.offerLast(i);
        }
    }
    int count = 0;
    while(!deque.isEmpty()){
        int cur = deque.pollFirst();
        count++;
        for(int nei : grapah.getOrDefault(cur, new ArrayList<>())){
            if(--indegree[nei] == 0){
                deque.offerLast(nei);
            }
        }
    }
    return count == N;
}
```

## [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

There are a total of `numCourses` courses you have to take, labeled from 0 to numCourses - 1. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

- For example, the pair`[0, 1]`, indicates that to take course 0 you have to first take course 1.

Return `true` if you can finish all courses. Otherwise, return false.

**Example 1:**

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0. So it is possible.

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        int[] inde = new int[numCourses];
        for(int[] c : prerequisites){
            map.computeIfAbsent(c[1], x -> new ArrayList<>()).add(c[0]);
            inde[c[0]]++;
        }
        Deque<Integer> deque = new ArrayDeque<>();
        for(int i = 0; i < numCourses; i++){
            if(inde[i] == 0){
                deque.addLast(i);
            }
        }
        int n = 0;
        while(!deque.isEmpty()){
            int cur = deque.pollFirst();
            for(int i : map.getOrDefault(cur, new ArrayList<>())){
                inde[i]--;
                if(inde[i] == 0){
                    deque.addLast(i);
                }
            }
            n++;
        }
        return n == numCourses;
    }
}
```

## [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

There are a total of `numCourses` courses you have to take, labeled from 0 to numCourses - 1. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

- For example, the pair`[0, 1]`, indicates that to take course 0 you have to first take course 1.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

**Example 1:**

Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
Same as 207, the only one thing we need to do is init an array to store the courses

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] res = new int[numCourses];
        Map<Integer, List<Integer>> map = new HashMap<>();
        int[] inde = new int[numCourses];
        for(int[] c : prerequisites){
            map.computeIfAbsent(c[1], x -> new ArrayList<>()).add(c[0]);
            inde[c[0]]++;
        }
        Deque<Integer> deque = new ArrayDeque<>();
        for(int i = 0; i < numCourses; i++){
            if(inde[i] == 0){
                deque.offerLast(i);
            }
        }
        int n = 0;
        while(!deque.isEmpty()){
            int pre = deque.pollFirst();
            res[n++] = pre;
            for(int i : map.getOrDefault(pre, new ArrayList<>())){
                inde[i]--;
                if(inde[i] == 0){
                    deque.offerLast(i);
                }
            }
        }
        return n == numCourses ? res : new int[0];
    }
}
```

## [269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)

There is a new alien language that uses the English alphabet. However, the order among the letters is unknown to you.

You are given a list of strings words from the alien language's dictionary, where the strings in words are sorted lexicographically by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there is no solution, return "". If there are multiple solutions, return any of them.

A string s is lexicographically smaller than a string t if at the first letter where they differ, the letter in s comes before the letter in t in the alien language. If the first `min(s.length, t.length)` letters are the same, then s is smaller if and only if `s.length < t.length`.

**Example 1:**

Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210624114833.png)

1. initialize`Map<Character, List<Character>> map`,`Map<Character, Integer> indegree`
2. iterate all the words, compare each char with the same index in two words (special case: if`word1.length() > word2.length() && word1.startsWith(word2)`, return directly)

```java
class Solution {
    public String alienOrder(String[] words) {
        Map<Character, List<Character>> map = new HashMap<>();
        Map<Character, Integer> indegree = new HashMap<>();
        String res = "";
        for(String word : words){
            for(char c : word.toCharArray()){
                map.put(c, new ArrayList<>());
                indegree.put(c, 0);
            }
        }
        for(int i = 0; i < words.length - 1; i++){
            String word1 = words[i];
            String word2 = words[i + 1];
            if(word1.length() > word2.length() && word1.startsWith(word2)){
                return "";
            }
            int len = Math.min(word1.length(), word2.length());
            for(int j = 0; j < len; j++){
                if(word1.charAt(j) != word2.charAt(j)){
                    map.get(word1.charAt(j)).add(word2.charAt(j));
                    indegree.put(word2.charAt(j), indegree.get(word2.charAt(j)) + 1);
                    break;
                }
            }
        }
        Deque<Character> deque = new ArrayDeque<>();
        for(char c : indegree.keySet()){
            if(indegree.get(c) == 0){
                deque.offerLast(c);
            }
        }
        while(!deque.isEmpty()){
            char c = deque.pollFirst();
            res += c;
            for(char suc : map.get(c)){
                indegree.put(suc, indegree.get(suc) - 1);
                if(indegree.get(suc) == 0){
                    deque.offerLast(suc);
                }
            }
        }
        return res.length() == indegree.size() ? res : new String("");
    
    }
}
```

# Sliding Window

滑动窗口算法可以用以解决数组/字符串的子元素问题，它可以将嵌套的循环问题，转换为单循环问题，降低时间复杂度。

如何识别滑动窗口？
1. 连续的元素，比如string, subarray, LinkedList
2. min, max, longest, shortest, key word

滑动窗口基本题型：
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210805110328.png)
## 3 Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
            Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

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

```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

- map存的 t 的 char 和 freq，理解成凑成我们需要的substring还需要的元素
- 

```java
class Solution {
    public String minWindow(String s, String t) {
        // map存的其实是我们sliding window里面还需要的元素们，这样理解的话就会清晰的多
        Map<Character, Integer> map = new HashMap<>();
        int left = 0;
        int res = 0;
        int n = s.length();
        int minLen = Integer.MAX_VALUE;
        int minLeft = 0;
        int count = 0;
        for(int i = 0; i < t.length(); i++){
            char c = t.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        for(int i = 0; i < n; i++){
            
            char c = s.charAt(i);
            if(map.containsKey(c)){
                // 这里map.get(c) > 0表示的是：符合t中char的个数的才算数，超过的其实不算数
                // 举个例子，s = "AAAA" t = "A" 这里其实substring中只要有一个A就是合格的，剩下的A都是无用功
                if(map.get(c) > 0){
                    count++;
                }
                // 既然出现了，那么我们还需要的元素就 - 1
                map.put(c, map.get(c) - 1);
            }
            
            while(count == t.length()){
                if(i - left + 1 < minLen){
                    minLen = i - left + 1;
                    minLeft = left;
                }
                char cur = s.charAt(left);
                if(map.containsKey(cur)){
                    // 这里代表我们把s.charAt(left) 从 substring踢出去
                    // 如果踢出去的是我们正好需要的，那么代表map里存的我们需要的元素个数变多了
                    map.put(cur, map.get(cur) + 1);
                    //  > 0 一定需要
                    if(map.get(cur) > 0){
                        count--;
                    }
                }
                left++;     
            }
        }
        return minLen == Integer.MAX_VALUE ? "" : s.substring(minLeft, minLeft + minLen);
    }
}
```

```java
class Solution {
    public String minWindow(String s, String t) {
        if (s == null || s == "" || t == null || t == "" || s.length() < t.length()) {
            return "";
        }
        //维护两个数组，记录已有字符串指定字符的出现次数，和目标字符串指定字符的出现次数
        //ASCII 表总长 128
        int[] need = new int[128];
        int[] have = new int[128];

        //将目标字符串指定字符的出现次数记录
        for (int i = 0; i < t.length(); i++) {
            need[t.charAt(i)]++;
        }

        //分别为左指针，右指针，最小长度（初始值为一定不可达到的长度）
        //已有字符串中目标字符串指定字符的出现总频次以及最小覆盖子串在原字符串中的起始位置
        int left = 0, right = 0, min = s.length() + 1, count = 0, start = 0;
        while (right < s.length()) {
            char r = s.charAt(right);
            //说明该字符不被目标字符串需要，此时有两种情况
            // 1. 循环刚开始，那么直接移动右指针即可，不需要做多余判断
            // 2. 循环已经开始一段时间，此处又有两种情况
            //  2.1 上一次条件不满足，已有字符串指定字出现次数不满足目标字符串指定字符出现次数，那么此时
            //      如果该字符还不被目标字符串需要，就不需要进行多余判断，右指针移动即可
            //  2.2 左指针已经移动完毕，那么此时就相当于循环刚开始，同理直接移动右指针
            if (need[r] == 0) {
                right++;
                continue;
            }
            //当且仅当已有字符串目标字符出现的次数小于目标字符串字符的出现次数时，count 才会+1
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
                //就不满足覆盖子串的条件，此时要破坏循环条件跳出循环，即控制目标字符串指定字符的出现总频次 (count）-1
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
## [159. Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)

Given a string s, return the length of the longest substring that contains at most two distinct characters.

**Example 1:**

    Input: s = "eceba"
    Output: 3
    Explanation: The substring is "ece" which its length is 3.

- **at most**
- use map to store the freq of each character

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        int n = s.length();
        int left = 0, res = 0;
        Map<Character, Integer> map = new HashMap<>();
        for(int i = 0; i < n; i++){
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
            while(map.size() > 2){
                char tmp = s.charAt(left);
                map.put(tmp, map.get(tmp) - 1);
                if(map.get(tmp) == 0){
                    map.remove(tmp);
                }
                left++;
            }
            res = Math.max(res, i - left + 1);
        }
        return res;
    }
}
```

## 220 Contains Duplicate III

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```

**Example 3:**

```
Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false
```

- [LeetCode 讲解](https://leetcode-cn.com/problems/contains-duplicate-iii/solution/hua-dong-chuang-kou-er-fen-sou-suo-shu-zhao-shang-/)

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

```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

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

```
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
```

思路：Brute Force 或者 [使用 Deque 双端队列](https://leetcode-cn.com/problems/sliding-window-maximum/solution/zui-da-suo-yin-dui-shuang-duan-dui-lie-cun-suo-yin/)

最简单直接的方法是遍历每个滑动窗口，找到每个窗口的最大值。一共有 N - k + 1 个滑动窗口，每个有 k 个元素，于是算法的时间复杂度为 {O}(N k)O(Nk)，表现较差。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        if (n * k == 0) return new int[0];
  
        int [] output = new int[n - k + 1];
        for (int i = 0; i < n - k + 1; i++) {
            int max = Integer.MIN_VALUE;
            for(int j = i; j < i + k; j++) 
                max = Math.max(max, nums[j]);
            output[i] = max;
        }
        return output;
    }
}
```

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
            while(!deque.isEmpty() && deque.peekFirst() == i - k){
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
            // i >= k -1 代表窗口长度已经满足要求了，而不是初始阶段 
            if(i >= k - 1){
                res[i - k + 1] = nums[deque.peekFirst];
            }
        }
        return res;
    }
}
```
## [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

Given a string s and an integer k, return the length of the longest substring of s that contains at most k distinct characters.

**Example 1:**

    Input: s = "eceba", k = 2
    Output: 3
    Explanation: The substring is "ece" with length 3.

- same with last problem

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        Map<Character, Integer> map = new HashMap<>();
        int left = 0, res = 0, n = s.length();
        for(int right = 0; right < n; right++){
            char c = s.charAt(right);
            map.put(c, map.getOrDefault(c, 0) + 1);
            while(map.size() > k){
                char tmp = s.charAt(left);
                map.put(tmp, map.get(tmp) - 1);
                if(map.get(tmp) == 0){
                    map.remove(tmp);
                }
                left++;
            }
            res = Math.max(res, right - left + 1);
        }
        return res;
    }
}
```

## [395. Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)

Given a string s and an integer k, return the length of the longest substring of s such that the frequency of each character in this substring is greater than or equal to k.

 

**Example 1:**

    Input: s = "aaabb", k = 3
    Output: 3
    Explanation: The longest substring is "aaa", as 'a' is repeated 3 times.

- brute force 26 alphabers
- then same with last question

```java
class Solution {
    public int longestSubstring(String s, int k) {
        int res = 0;
        for(int num = 0; num <= 26; num++){
            Map<Character, Integer> map = new HashMap<>();
            int left = 0;
            int validCnt = 0;
            for(int i = 0; i < s.length(); i++){
                char c = s.charAt(i);
                map.put(c, map.getOrDefault(c, 0) + 1);
                if(map.get(c) == k) validCnt++;
                while(map.size() > num){
                    char tmp = s.charAt(left);
                    if(map.get(tmp) == k) validCnt--;
                    map.put(tmp, map.get(tmp)-1);
                    if(map.get(tmp) == 0) map.remove(tmp);
                    left++;
                }
                if(validCnt == num) res = Math.max(res, i - left + 1);
            }
        }
        return res;
    }
}
```
## 424 Longest Repeating Character Replacement

Given a string s that consists of only uppercase English letters, you can perform at most k operations on that string.

In one operation, you can choose any character of the string and change it to any other uppercase English character.

Find the length of the longest sub-string containing all repeating letters you can get after performing the above operations.

Note:
Both the string's length and k will not exceed 104.

**Example 1:**

```
Input:
s = "ABAB", k = 2

Output:
4

Explanation:
Replace the two 'A's with two 'B's or vice versa.
```

**Example 2:**

```
Input:
s = "AABABBA", k = 1

Output:
4

Explanation:
Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
```

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int left = 0, right = 0;
        int N = s.length();
        int[] count = new int[28];
        int max = 0;
        int res = 0;
        while(right < N){
            count[s.charAt(right) - 'A']++;
            max = Math.max(max, count[s.charAt(right) - 'A']);
            while(right - left + 1 - max > k){
                count[s.charAt(left) - 'A']--;
                left++;
            }
            res = Math.max(res, right - left + 1);
            right++;
        }
        return res;
    }
}
```

## 567 Permutation in String

Given two strings s1 and s2, write a function to return true if s2 contains the permutation of s1. In other words, one of the first string's permutations is the substring of the second string.

**Example 1:**

```
Input: s1 = "ab" s2 = "eidbaooo"
Output: True
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**

```
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```

```java
class Solution {
    public boolean matches(int[] s1map, int[] s2map) {
        for (int i = 0; i < 26; i++) {
            if (s1map[i] != s2map[i])
                return false;
        }
        return true;
    }

    public boolean checkInclusion(String s1, String s2) {
        int N1 = s1.length();
        int N2 = s2.length();
        int[] c1 = new int[26];
        int[] c2 = new int[26];
  
        if(N1 > N2) return false;
  
        for(int i = 0; i < N1; i++){
            c1[s1.charAt(i) - 'a']++;
            c2[s2.charAt(i) - 'a']++;
        }
  
        for(int left = 0; left < N2 - N1; left++){
            for(int i = 0; i < 26; i++){
                if(matches(c1, c2)) {
                    return true;
                }
            }
            c2[s2.charAt(left) - 'a']--;
            c2[s2.charAt(left + N1) - 'a']++;
  
        }  
        return matches(c1, c2);
    }
}
```

## 713 Subarray Product Less Than K

Your are given an array of positive integers `nums`.

Count and print the number of (contiguous) subarrays where the product of all the elements in the subarray is less than `k`.

**Example 1:**

```
Input: nums = [10, 5, 2, 6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are: [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6].
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```

**Note:**

```
0 < nums.length <= 50000.
0 < nums[i] < 1000.
0 <= k < 10^6.
```

最后我们为什么采用这个等式呢？

`ans += r - l + 1`

```
对于一个长度为 len 的连续的满足条件的子数组，我们可以把它进行细分
第一次，细分为 1 个单位长度，可以得到 len 个
第二次，细分为 2 个单位长度，可以得到 len-1 个
......
第 len 次，细分为 len 个单位长度，可以得到 1 个
总共有 1+2+...+len-1+len 个
```

当 r 右移时，若乘积超出范围了，我们需要从左边摘除一部分数值使得乘积重新满足条件，需要摘多少？由于 nums 中元素均为正整数，所以乘积必定大于等于 1, 而题目要求不能取等号，所以最坏的情况是 l=r+1, 即左指针移到了右指针右边，此时 ans+=0

如果经过摘除左边元素后 l 依然小于 r, 怎么推出 `ans+=r-l+1`

```
r 左边的元素均已经考虑了所有的组合，所以我们只要考虑含 r 的组合，显然含 r 的组合数刚好是子数组的长度
举个例子 [...5,6,3,4,8,...] 假设 r 遍历到了数值 8 的位置，l 经过摘除后移到了数值 5 处，
此时所有的组合情况是：
[56348]
[6348]
[348]
[48]
[8]
即 5 种
```

具体实现时，需要注意俩个细节

`if k<=1: return 0`
`while l<=r and product >= k:`

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int N = nums.length;
        int count = 0;
        int product= 1;
        int left = 0, right = 0;
        if(k == 0) return 0;
        if(k == 1) return 0;
        while(right < N){
            product *= nums[right];
            while(left <= right && product >= k){
                product /= nums[left];
                left++;
            }
            count += right - left + 1;
            right++;
        }
        return count;
    }
}
```

# Monotonic Stack

## 数组中每个数右边比它大的第一个数

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210729111422.png)

```java
private static int[] nextGreaterElement(int[] nums){
    int[] res = new int[nums.length];
    Deque<Integer> stack = new ArrayDeque<>();
    for(int i = nums.length - 1; i >= 0; i--){
        while(!stack.isEmpty() && nums[i] >= stack.peekLast()){
            stack.removeLast();
        }
        res[i] = stack.isEmpty() ? -1 : stack.peekLast();
        stack.addLast(nums[i]);
    }
    return res;
}
```
## [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

The next greater element of some element x in an array is the first greater element that is to the right of x in the same array.

You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2.

For each 0 <= i < nums1.length, find the index j such that `nums1[i] == nums2[j]` and determine the next greater element of `nums2[j]` in nums2. If there is no next greater element, then the answer for this query is -1.

Return an array ans of length nums1.length such that `ans[i]` is the next greater element as described above.

 

**Example 1:**

    Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
    Output: [-1,3,-1]
    Explanation: The next greater element for each value of nums1 is as follows:
    - 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
    - 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
    - 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

**same as last question, only one thing we need to do is use a HashMao to cache the relationship between index and the changed value.**

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] res = new int[nums1.length];
        Map<Integer, Integer> map = new HashMap<>();
        Deque<Integer> deque = new ArrayDeque<>();
        for(int i = nums2.length - 1; i >= 0; i--){
            while(!deque.isEmpty() && nums2[i] >= deque.peekLast()){
                deque.removeLast();
            }
            map.put(nums2[i], deque.isEmpty() ? -1 : deque.peekLast());
            deque.addLast(nums2[i]);
        }
        for(int i = 0; i < nums1.length; i++){
            res[i] = map.get(nums1[i]);
        }
        return res;
    }
}
```

## [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

> Same as Leetcode [1081. Smallest Subsequence of Distinct Characters](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

Given a string s, remove duplicate letters so that every letter appears once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

**Example 1:**

    Input: s = "bcabc"
    Output: "abc"

**Example 2:**

    Input: s = "cbacdcbc"
    Output: "acdb"
    
- use `int[] last = new int[128]` to store the last index of the character
- **key step:** 
  - `!deque.isEmpty()`
  -  cur char < deque.peek()
  -  deque.peek() is not the last one: which means we could drop the peek() one, cuz we still have one in the later

```java
class Solution {
    public String smallestSubsequence(String s) {
         Deque<Integer> deque = new ArrayDeque<>();
        int[] last = new int[128];
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < s.length(); i++){
            last[s.charAt(i)] = i;
        }
        for(int i = 0; i < s.length(); i++){
            int c = s.charAt(i);
            if(!set.contains(c)){
                set.add(c);
                while(!deque.isEmpty() && c < deque.peekLast() && i < last[deque.peekLast()]){
                    set.remove(deque.removeLast());
                }
                deque.addLast(c);
            }

        }
        StringBuilder sb = new StringBuilder();
        for(int i : deque){
            sb.append((char) i);
        }
        return sb.toString();
    }
}
```
## [402. Remove K Digits](https://leetcode.com/problems/remove-k-digits/)

Given string num representing a non-negative integer num, and an integer k, return the smallest possible integer after removing k digits from num.

**Example 1:**

    Input: num = "1432219", k = 3
    Output: "1219"
    Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

- same idea with `316. Remove Duplicate Letters`
- for monotonic stack part:
  - `!deqeue.isEmpty()`
  - k > 0
  - cur value < deque.peekLast()
- **ATTENTION:** there is a possible that the number only have 1 digit, like num = "9", k = 1, in that case, we would not run the monotonic part, instead, it would be directly push into stack. so after for loop, we still need to remove the element in k times
- **ATTENTION:** after processing, we need to remove the meannigless 0 element in the front
- **ATTENTION:** if res is "", we should return 0, not null String.


```java
class Solution {
    public String removeKdigits(String num, int k) {
        Deque<Character> deque = new ArrayDeque<>();
        for(int i = 0; i < num.length(); i++){
            char c = num.charAt(i);
            while(!deque.isEmpty() && k > 0 && c < deque.peekLast()){  
                deque.removeLast();
                k--;
            }
            deque.addLast(c);
        }
        while(k-- > 0){
            deque.removeLast();
        }
        StringBuilder sb = new StringBuilder();
        boolean zero = true;
        for(char c : deque){
            if(zero && c == '0'){
                continue;
            }else{
                zero = false;
            }
            sb.append(c);
        }
        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```  

## [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

    Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
    Output: 6
    Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

- 单调栈解法，首先一直往栈里压墙的高度，只有高度从低变高的时候我们才需要计算储水，高度递减的时候我们不需要管，因为这时候不储存水量
- 如果当前高度大于栈顶的高度，我们先pop出栈顶的元素保存起来，这个是我们的水池底部
- 如果这时候栈空了，直接break，因为没有左边的墙了，此时蓄不住水
- 求出当前水池最矮的墙`Math.min(height[deque.peekLast()], height[i])`，
- 高度*长度求出当前level的蓄水量，循环往复
- 不要忘记往栈里压入当前墙

```java
class Solution {
    public int trap(int[] height) {
        Deque<Integer> deque = new ArrayDeque<>();
        int n = height.length;
        int res = 0;
        for(int i = 0; i < n; i++){
            while(!deque.isEmpty() && height[i] > height[deque.peekLast()]){
                int pre = deque.removeLast();
                if(deque.isEmpty()) break;
                int minHeight = Math.min(height[deque.peekLast()], height[i]);
                res += (minHeight - height[pre]) * (i - deque.peekLast() - 1);
            }
            deque.addLast(i);
        }
        return res;
    }
}
```

## [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

    Input: heights = [2,1,5,6,2,3]
    Output: 10
    Explanation: The above is a histogram where width of each bar is 1.
    The largest rectangle is shown in the red area, which has an area = 10 units.

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210802223849.png)

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        Deque<Integer> deque = new ArrayDeque<>();
        int res = 0;
        int n = heights.length;
        for(int i = 0; i < n; i++){
            while(!deque.isEmpty() && heights[i] <= heights[deque.peekLast()]){
                int pre = heights[deque.removeLast()];
                int width = i - (deque.isEmpty() ? 0 : deque.peekLast() + 1);
                res = Math.max(res, pre * width);
            }
            deque.addLast(i);
        }
        while(!deque.isEmpty()){
            int pre = heights[deque.removeLast()];
            int width = heights.length - (deque.isEmpty() ? 0 : deque.peekLast() + 1);
            res = Math.max(res, pre * width);
        }
        return res;
    }
}
```

# Monotonic Queue

什么是单调队列？

单调队列，顾名思义其中所有的元素都是单调的(递增或者递减)，承载的基础数据结构是队列，实现是双端队列，队列中存入的元素为数组索引，队头元素为窗口的最大(最小)元素。

**队头删除不符合有效窗口的元素，队尾删除不符合最值的候选元素。**

单调队列不是真正的队列。因为队列都是FIFO的，统一从队尾入列，从对首出列。但单调队列是从队尾入列，从队首或队尾出列，所以单调队列不遵守FIFO。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210803111228.png)

如何实现单调队列？

`Deque<Integer> q = new ArrayDeque()`

如何使用单调队列？

一般能用单调队列的题目也能用PQ去写，只不过单调队列是pq的优化解。

模版如下：
1. **去尾操作：队尾元素出队列。**当队列有新元素待入队，需要从队尾开始，删除影响队列单调性的元素，维护队列的单调性。（删除一个队尾元素后，就重新判断新的队尾元素）
2. 去尾操作结束后，将该新元素重新入队列
3. **删头操作：队头元素出队列。**判断队头元素是否在待求解的区间之内，如果不在，就将其删除。（这个很好理解，因为单调队列的队头元素就是待求解区间的极值）
4. **取解操作：**经过上面两个操作，取出**队列的头元素**，就是**当前区间的极值**。

## [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

跳转：在Sliding Window中也讲过，[点击跳转](##239-Sliding-Window-Maximum)

## [862. Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)

Given an integer array nums and an integer k, return the length of the shortest non-empty subarray of nums with a sum of at least k. If there is no such subarray, return -1.

A subarray is a contiguous part of an array.

**Example 1:**

    Input: nums = [1], k = 1
    Output: 1

**Example 2:**

    Input: nums = [1,2], k = 4
    Output: -1

- preSum is necessary
- `j > i && preSum[i] - preSum[j] >= k && (j - i)最小` 
- when the first `sum[i] - sum[deque.peekFirst()] >= k` comes out, we do not have to calculate the rest result, cuz we need the shortest subarray 

```java
class Solution {
    public int shortestSubarray(int[] nums, int k) {
        int n = nums.length;
        int res = n + 1;
        int[] sum = new int[n + 1];
        for(int i = 0; i < n; i++){
            sum[i + 1] = nums[i] + sum[i];
        }
        Deque<Integer> deque = new ArrayDeque<>();
        for(int i = 0; i < sum.length; i++){
            while(!deque.isEmpty() && sum[i] - sum[deque.peekFirst()] >= k){
                int start = deque.removeFirst();
                res = Math.min(res, i - start);
            }
            while(!deque.isEmpty() && sum[i] <= sum[deque.peekLast()]){
                deque.removeLast();
            }
            deque.addLast(i);
        }
        return res <= n ? res : -1;
    }
}
```
## [1425. Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/)

Given an integer array nums and an integer k, return the maximum sum of a non-empty subsequence of that array such that for every two consecutive integers in the subsequence, `nums[i]` and `nums[j]`, where `i < j`, the condition `j - i <= k` is satisfied.

A subsequence of an array is obtained by deleting some number of elements (can be zero) from the array, leaving the remaining elements in their original order.

**Example 1:**

    Input: nums = [10,2,-10,5,20], k = 2
    Output: 37
    Explanation: The subsequence is [10, 2, 5, 20].

- sum[i] means local max sum till i;

```java
class Solution {
    public int constrainedSubsetSum(int[] nums, int k) {
        int n = nums.length;
        Deque<Integer> deque = new ArrayDeque<>();
        int[] sum = new int[n];
        int res = nums[0];
        for(int i = 0; i < n; i++){
            sum[i] = nums[i];
            if(!deque.isEmpty()){
                sum[i] += sum[deque.peekFirst()];
            }
            res = Math.max(res, sum[i]);
            while(!deque.isEmpty() && i - deque.peekFirst() >= k){
                deque.removeFirst();
            }
            while(!deque.isEmpty() && sum[i] >= sum[deque.peekLast()]){
                deque.removeLast();
            }
            if(sum[i] > 0){
                deque.addLast(i);
            }
        }
        return res;
    }
}
```

## [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)


Given an array of integers nums and an integer limit, return the size of the longest non-empty subarray such that the absolute difference between any two elements of this subarray is less than or equal to limit.

 

**Example 1:**

    Input: nums = [8,2,4,7], limit = 4
    Output: 2 
    Explanation: All subarrays are: 
    [8] with maximum absolute diff |8-8| = 0 <= 4.
    [8,2] with maximum absolute diff |8-2| = 6 > 4. 
    [8,2,4] with maximum absolute diff |8-2| = 6 > 4.
    [8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
    [2] with maximum absolute diff |2-2| = 0 <= 4.
    [2,4] with maximum absolute diff |2-4| = 2 <= 4.
    [2,4,7] with maximum absolute diff |2-7| = 5 > 4.
    [4] with maximum absolute diff |4-4| = 0 <= 4.
    [4,7] with maximum absolute diff |4-7| = 3 <= 4.
    [7] with maximum absolute diff |7-7| = 0 <= 4. 
    Therefore, the size of the longest subarray is 2.

- 2 deque: ascending deque / descending deque
- maintain the monotonism of 2 deqeus
- if max(`maxDeque.peekFirst()`) - min(`minDeque.peekFirst()`) > limit, which means we need shrink the range, use left pointer to control.

```java
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        Deque<Integer> maxd = new ArrayDeque<>();
        Deque<Integer> mind = new ArrayDeque<>();
        int left = 0;
        int res = 0;
        for(int i = 0;i < nums.length; i++){
            while(!maxd.isEmpty() && maxd.peekLast() < nums[i]){
                maxd.removeLast();
            }
            while(!mind.isEmpty() && mind.peekLast() > nums[i]){
                mind.removeLast();
            }
            maxd.addLast(nums[i]);
            mind.addLast(nums[i]);
            if(maxd.peekFirst() - mind.peekFirst() > limit){
                if(maxd.peekFirst() == nums[left]) maxd.removeFirst();
                if(mind.peekFirst() == nums[left]) mind.removeFirst();
                left++;
            }
            res = Math.max(res, i - left + 1);
        }
        return res;
    }
}
```

## [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

You are given a 0-indexed integer array nums and an integer `k`.

You are initially standing at index 0. In one move, you can jump at most k steps forward without going outside the boundaries of the array. That is, you can jump from index i to any index in the range `[i + 1, min(n - 1, i + k)]` inclusive.

You want to reach the last index of the array (index n - 1). Your score is the sum of all `nums[j]` for each index `j` you visited in the array.

Return the **maximum score** you can get.

**Example 1:**

    Input: nums = [1,-1,-2,4,-7,3], k = 2
    Output: 7
    Explanation: You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210804214857.png)

- actually this problem's core is dp
- in the inner 2 while loop, why we use >= and <=?
  - cuz cur result is in the range k - 1, after deque.addLast(i), it should in the correct range k

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210804220254.png)

```java
class Solution {
    public int maxResult(int[] nums, int k) {
        Deque<Integer> deque = new ArrayDeque<>();
        int n = nums.length;
        int[] dp = new int[n];
        dp[0] = nums[0];
        deque.addLast(0);
        for(int i = 0; i < n - 1; i++){
            while(!deque.isEmpty() && i - deque.peekFirst() >= k){
                deque.removeFirst();
            }
            while(!deque.isEmpty() && dp[deque.peekLast()] <= dp[i]){
                deque.removeLast();
            }
            deque.addLast(i);
            dp[i + 1] = dp[deque.peekFirst()] + nums[i + 1];
        }
        return dp[n - 1];
    }
}
```

# Tree 

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210708111855.png)

## Binary Tree Traversal

### preorder

**recursive**

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        pre(root, res);
        return res;
    
    }
    private void pre(TreeNode node, List<Integer> res){
        if(node == null){
            return;
        }
        res.add(node.val);
        pre(node.left, res);
        pre(node.right, res);
    }
}
```

**iterative**

method1:

```java
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        if(root != null) stack.push(root);

        while(!stack.isEmpty()){
            TreeNode node = stack.pop();
            list.add(node.val);
            // Remember to reverse order.. right -> left
            if(node.right != null) stack.push(node.right);
            if(node.left != null) stack.push(node.left);
        }

        return list;
    }
}
```
method2:
```java
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        TreeNode cur = root;
        while(cur != null || !deque.isEmpty()){
            if(cur != null){
                res.add(cur.val); // root
                deque.addLast(cur.val);
                cur = cur.left; // left
            }else{
                cur = stack.removeLast();
                cur = cur.right; // right
            }
        }
    }
}
```

### inorder
**recursive**
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        pre(root, res);
        return res;
    
    }
    private void pre(TreeNode node, List<Integer> res){
        if(node == null){
            return;
        }
        pre(node.left, res);
        res.add(node.val);
        pre(node.right, res);
    }
}
```
**iterative**
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        while(root != null || !deque.isEmpty()){
            while(root != null){
                deque.addLast(root);
                root = root.left;
            }
            root = deque.removeLast();
            res.add(root.val);
            root = root.right;
        }
        return res;
    }
}
```
### postorder
**recursive**
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        pre(root, res);
        return res;
    
    }
    private void pre(TreeNode node, List<Integer> res){
        if(node == null){
            return;
        }
        pre(node.left, res);
        pre(node.right, res);
        res.add(node.val);
    }
}
```
**iterative**
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210709171431.png)
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        if(root != null) deque.addLast(root);
        while(!deque.isEmpty()){
            TreeNode tmp = deque.removeLast();
            res.add(0, tmp.val);
            if(tmp.left != null){
                deque.addLast(tmp.left);
            }
            if(tmp.right != null){
                deque.addLast(tmp.right);
            }
        }
        return res;
    }
}
```

### [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        if(root != null){
            deque.addLast(root);
        }
        while(!deque.isEmpty()){
            List<Integer> path = new ArrayList<>();
            int size = deque.size();
            for(int i = 0; i < size; i++){
                TreeNode tmp = deque.removeFirst();
                path.add(tmp.val);
                if(tmp.left != null) deque.addLast(tmp.left);
                if(tmp.right != null) deque.addLast(tmp.right);              
            }
            res.add(path);
        }
        return res;
    }
}
```

### [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)

level order from leaf to root

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        if(root != null) deque.addLast(root);
        while(!deque.isEmpty()){
            List<Integer> path = new ArrayList<>();
            int size = deque.size();
            for(int i = 0; i < size; i++){
                TreeNode tmp = deque.removeFirst();
                path.add(tmp.val);
                if(tmp.left != null) deque.addLast(tmp.left);
                if(tmp.right != null) deque.addLast(tmp.right);
            }
            res.add(0, path);
        }
        return res;
    }
}
```

### [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

use a flag(boolean) to determine weather we need to change the order
after added, reverse the falg's value

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        if(root != null) deque.addLast(root);
        boolean flag = true;
        while(!deque.isEmpty()){
            int size = deque.size(); 
            List<Integer> path = new ArrayList<>();
            for(int i = 0; i < size; i++){
                TreeNode cur = deque.removeFirst();
                if(flag) path.add(cur.val);
                else path.add(0, cur.val);
                if(cur.left != null) deque.addLast(cur.left);
                if(cur.right != null) deque.addLast(cur.right);
            }
            flag = !flag;
            res.add(path);
        }
        return res;
    }
}
```

### [314. Binary Tree Vertical Order Traversal](https://leetcode.com/problems/binary-tree-vertical-order-traversal/)

`Map<Integer, List<Integer>> col` record the mapping relationship between the column and the nodes in that column
`Map<TreeNode, Integer> map` record the single node and its column

```java
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        Deque<TreeNode> deque = new ArrayDeque<>();
        Map<Integer, List<Integer>> col = new HashMap<>();
        Map<TreeNode, Integer> map = new HashMap<>();
        if(root == null) return res;
        deque.addLast(root); 
        map.put(root, 0);
        int left = 0, right = 0;
        while(!deque.isEmpty()){
            TreeNode cur = deque.removeFirst();
            int i = map.get(cur);
            col.computeIfAbsent(i, x -> new ArrayList<>()).add(cur.val);
            if(cur.left != null){
                deque.addLast(cur.left);
                map.put(cur.left, i - 1);
            }
            if(cur.right != null){
                deque.addLast(cur.right);
                map.put(cur.right, i + 1);
            }
            left = Math.min(left, i);
            right = Math.max(right, i);
        }
        for(int j = left; j <= right; j++){
            res.add(col.get(j));
        }
        return res;
    }
}
```

## 结构转化+序列化

### [297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

```java
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null) return "null";
        return root.val + "," + serialize(root.left) + "," + serialize(root.right);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        Deque<String> deque = new ArrayDeque<>(Arrays.asList(data.split(",")));
        return help(deque);
    }
    private TreeNode help(Deque<String> deque){
        String s = deque.removeFirst();
        if(s.equals("null")) return null;
        TreeNode root = new TreeNode(Integer.valueOf(s));
        root.left = help(deque);
        root.right = help(deque);
        return root;
    }
}
```
### [428. Serialize and Deserialize N-ary Tree](https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree/)

store the node.val and its children size into the ArrayList at the same time

```java
class Codec {
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        List<String> res = new ArrayList<>();
        dfs(root, res);
        return String.join(",", res);
        
    }
    private void dfs(Node root, List<String> res){
        if(root == null) return;
        res.add(String.valueOf(root.val));
        res.add(String.valueOf(root.children.size()));
        for(Node node : root.children){
            dfs(node, res);
        }
    }
	
    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        if(data.equals("")) return null;
        Deque<String> deque = new ArrayDeque<>(Arrays.asList(data.split(",")));
        return help(deque);
        
    }
    private Node help(Deque<String> deque){
        Node root = new Node(Integer.valueOf(deque.removeFirst()), new ArrayList<>());
        int size = Integer.valueOf(deque.removeFirst());
        for(int i = 0; i < size; i++){
            root.children.add(help(deque));
        }
       return root; 
    }
}
```

### [449. Serialize and Deserialize BST](https://leetcode.com/problems/serialize-and-deserialize-bst/)

```java
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        List<String> res = new ArrayList<>();
        dfs(root, res);
        return String.join(",", res);
        
        
    }
    private void dfs(TreeNode root, List<String> res){
        if(root == null) return;
        res.add(String.valueOf(root.val));
        if(root.left != null) dfs(root.left, res);
        if(root.right != null) dfs(root.right, res);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data.isEmpty()) return null;
        Deque<String> deque = new ArrayDeque<>(Arrays.asList(data.split(",")));
        return help(deque, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }
    private TreeNode help(Deque<String> deque, int low, int high){
        if(deque.isEmpty()) return null;
        int val = Integer.valueOf(deque.peekFirst());
        if(val > high || val < low) return null;
        deque.removeFirst();
        TreeNode root = new TreeNode(val);
        root.left = help(deque, low, val);
        root.right = help(deque, val, high);
        return root;
    }
}
```
### [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

```java
class Solution {
    int index = 0;
    public TreeNode bstFromPreorder(int[] preorder) {
        return help(preorder, Integer.MIN_VALUE, Integer.MAX_VALUE);
        
    }
    private TreeNode help(int[] preorder, int low, int high){
        if(index == preorder.length){
            return null;
        }
        int val = preorder[index];
        if(val < low || val > high) return null;
        TreeNode root = new TreeNode(val);
        index++;
        root.left = help(preorder, low, val);
        root.right = help(preorder, val, high);
        return root;
    }
}
```

### [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715222237.png)
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715222150.png)

### [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715222413.png)
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715222442.png)

利用postorder性质优化解法：

注意：这里是postorder，postorder是 **左 右 根** 的顺序，所以我们index需要从后往前，那我们recursion的顺序也变成了先right后left

```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();
    int pindex;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        pindex = inorder.length - 1;
        for(int i = 0; i < inorder.length; i++){
            map.put(inorder[i], i);
        }
        return help(postorder, 0, inorder.length - 1);
    }
    private TreeNode help(int[] postorder, int inlow, int inhigh){
        if(inlow > inhigh) return null;
        TreeNode root = new TreeNode(postorder[pindex]);
        int index = map.get(postorder[pindex]);
        pindex--;
        root.right = help(postorder, index + 1, inhigh);
        root.left = help(postorder, inlow, index - 1);
        return root;
    }
}
```
### [889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

基本思路是一样的，首先需要去找左子树的size这样才能划分下一个recursive的边界。因为preorder左子树第一个孩子，就是postorder左边子树的最后一个(因为左子树本身可能是parent，postorder最后才读parent)

为什么在这道题里我们需要单独加一个特判呢？因为我们这道题比之前的题多了一点，就是拿到root后我们需要往后再取一位找出左子树的节点，这样的话我们为了防止数组越界：
- 在第一张图中，我们用一个全局preStart遍历，加完之后要判断一下preStart == N 防止越界
- 在第二张图中，我们用prel == preRight 或者 postL == postR来进行判断


![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715223034.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715223101.png)

### [426. Convert Binary Search Tree to Sorted Doubly Linked List](https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/)

利用两个TreeNode指针操作，一个first记录开头，一个prev和current root进行交换

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715225614.png)


## BST
### [270. Closest Binary Search Tree Value](https://leetcode.com/problems/closest-binary-search-tree-value/)

```java
class Solution {
    double min = Double.MAX_VALUE;
    int res = 0;
    public int closestValue(TreeNode root, double target) {
        help(root, target);
        return res;
    }
    private void help(TreeNode root, double target){
        if(root == null) return;
        double diff = Math.abs(root.val - target);
        if(diff < min){
            min = diff;
            res = root.val;
        }
        if(root.val > target){
            help(root.left, target);
        }else{
            help(root.right, target);
        }
    }
}
```

### [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

需要注意几个情况：
- 节点是叶子结点
  - 直接删除即可
- 节点有一个孩子
  - 删除当前节点，把孩子直接接上来
- 节点有两个孩子
  - 删除当前节点，替换成左孩子的最大（最右的叶子结点）或者右孩子的最小（最左的叶子结点）

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null) return null;
        if(key < root.val){
            root.left = deleteNode(root.left, key);
        }else if(key > root.val){
            root.right = deleteNode(root.right, key);
        }else if(root.left == null){
            return root.right;
        }else if(root.right == null){
            return root.left;
        }else{
            root.val = findMin(root.right);
            root.right = deleteNode(root.right, root.val);
        }
        return root;
    }
    private int findMin(TreeNode root){
        while(root.left != null){
            root = root.left;
        }
        return root.val;
    }
}
```
### [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
三种做法：
```java
class Solution {
    TreeNode prev;
    public boolean isValidBST(TreeNode root) {
        Deque<TreeNode> deque = new ArrayDeque<>();
        while(root != null || !deque.isEmpty()){
            while(root != null){
                deque.addLast(root);
                root = root.left;
            }
            root = deque.removeLast();
            if(prev != null && root.val <= prev.val){
                return false;
            }
            prev = root;
            root = root.right;
        }
        return true;
    }
}
```
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return help(root, Long.MAX_VALUE, Long.MIN_VALUE);
    }
    private boolean help(TreeNode root, long max, long min){
        if(root == null) return true;
        if(root.val <= min || root.val >= max){
            return false;
        }
        return help(root.left, root.val, min) && help(root.right, max, root.val);
    }
}
```
```java
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        if(!isValidBST(root.left)) return false;
        if(pre >= root.val) return false;
        pre = root.val;
        if(!isValidBST(root.right)) return false;
        return true;       
    }
}
```
### [173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)
```java
class BSTIterator {
    Deque<TreeNode> deque;
    public BSTIterator(TreeNode root) {
        deque = new ArrayDeque<>();
        putLeft(root);
    }
    
    public int next() {
        TreeNode cur = deque.removeLast();
        putLeft(cur.right);
        return cur.val;
    }
    
    public boolean hasNext() {
        return !deque.isEmpty();
        
    }
    private void putLeft(TreeNode root){
        while(root != null){
            deque.addLast(root);
            root = root.left;
        }
    }
}
```

### [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/)

```java
class Solution {
    TreeNode first, second, prev;
    public void recoverTree(TreeNode root) {
        inorder(root);
        int tmp = first.val;
        first.val = second.val;
        second.val = tmp;
        
    }
    private void inorder(TreeNode root){
        if(root == null) return;
        inorder(root.left);
        if(prev != null && prev.val >= root.val){
            if(first == null){
                first = prev;
                second = root;
            }else{
                second = root;
            }
        }
        prev = root;
        inorder(root.right);
    }
}
```
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715234130.png)

### 108 Convert Sorted Array to Binary Search Tree

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

**Example:**

```
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

     0
    / \
  -3   9
  /   /
-10  5
```

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return dfs(nums, 0, nums.length - 1);
    }

    private TreeNode dfs(int[] nums, int lo, int hi) {
        if (lo > hi) {
            return null;
        } 
        // 以升序数组的中间元素作为根节点 root
        int mid = lo + (hi - lo) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        // 递归的构建 root 的左子树与右子树。
        root.left = dfs(nums, lo, mid - 1);
        root.right = dfs(nums, mid + 1, hi); 
        return root;
    }
}
```
### [1382. Balance a Binary Search Tree](https://leetcode.com/problems/balance-a-binary-search-tree/)
先inorder，再用上一题的方法
```java
class Solution {
    List<Integer> list = new ArrayList<>();
    public TreeNode balanceBST(TreeNode root) {
        inorder(root);
        return help(0, list.size() - 1);
    }
    private TreeNode help(int low, int high){
        if(low > high) return null;
        int mid = (low + high) / 2;
        TreeNode root = new TreeNode(list.get(mid));
        root.left = help(low, mid - 1);
        root.right = help(mid + 1, high);
        return root;
    }
    private void inorder(TreeNode root){
        if(root == null) return;
        inorder(root.left);
        list.add(root.val);
        inorder(root.right);
    }
}
```

### [96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210715234458.png)

```java
class Solution {
    public int numTrees(int n) {
        int[] G = new int[n + 1];
        G[0] = 1;
        G[1] = 1;
        for(int i = 2; i <= n; i++){
            for(int j = 1; j <= i; j++){
                G[i] += G[j - 1] * G[i - j];
            }
        }
        return G[n];
    }
}
```

### [95. Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if(n == 0) return new ArrayList<>();
        return help(1, n);
        
    }
    private List<TreeNode> help(int low, int high){
        List<TreeNode> res = new ArrayList<>();
        if(low > high) res.add(null);
        for(int i = low; i <= high; i++){
            List<TreeNode> left = help(low, i - 1);
            List<TreeNode> right = help(i + 1, high);
            for(TreeNode lNode : left){
                for(TreeNode rNode: right){
                    TreeNode root = new TreeNode(i);
                    root.left = lNode;
                    root.right = rNode;
                    res.add(root);
                }
            }
        }
        return res;
    }
}
```






## LCA(Lowest Common Ancestor)
### [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

如果当前节点的值大于p,q的值，证明p和q在当前节点的左边，那我们就往当前节点的左子树搜索，反之同理。
如果当前节点在p和q中间，那就找到了，直接返回即可。

we could use the BST's feature to solve the question:

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return root;
        if(root.val > p.val && root.val > q.val) return lowestCommonAncestor(root.left, p, q);
        if(root.val < p.val && root.val < q.val) return lowestCommonAncestor(root.right, p, q);
        return root;
    }
}
```

### 236 Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”

Given the following binary tree:  root = `[3,5,1,6,2,0,8,null,null,7,4]`

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200917001854.png)

**Example 1:**

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

思路：

- [二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/solution/er-cha-shu-de-zui-jin-gong-gong-zu-xian-by-leetc-2/)
- [二叉树的最近公共祖先（后序遍历 DFS ，清晰图解）](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/solution/236-er-cha-shu-de-zui-jin-gong-gong-zu-xian-hou-xu/)
- [java 代码递归和非递归图文详解](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/solution/javadai-ma-di-gui-he-fei-di-gui-tu-wen-xiang-jie-b/)

```java
public TreeNode lowestCommonAncestor(TreeNode cur, TreeNode p, TreeNode q) {
        if (cur == null || cur == p || cur == q)
            return cur;
        TreeNode left = lowestCommonAncestor(cur.left, p, q);
        TreeNode right = lowestCommonAncestor(cur.right, p, q);
        //如果 left 为空，说明这两个节点在 cur 结点的右子树上，我们只需要返回右子树查找的结果即可
        if (left == null)
            return right;
        //同上
        if (right == null)
            return left;
        //如果 left 和 right 都不为空，说明这两个节点一个在 cur 的左子树上一个在 cur 的右子树上，
        //我们只需要返回 cur 结点即可。
        return cur;
    }
```

### [1644. Lowest Common Ancestor of a Binary Tree II](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-ii/)

题目跟上一题一样，只不过区别在于，题目中给定的两个节点有可能是不存在的，不存在的话我们就要返回null

两种方法：

**method 1:** first, dfs this tree to determine whether those 2 nodes exist, then use the same method to find the lca

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode np = dfs(root, p);
        TreeNode nq = dfs(root, q);
        if(np == null || nq == null) return null;
        else return help(root, np, nq);
        
    }
    private TreeNode dfs(TreeNode root, TreeNode target){
        if(root == null) return null;
        if(root.val == target.val) return root;
        TreeNode left = dfs(root.left, target);
        TreeNode right = dfs(root.right, target);
        return left == null ? right : left;
    }
    private TreeNode help(TreeNode root, TreeNode p, TreeNode q){
        if(root == null || root == q || root == p) return root;
        TreeNode left = help(root.left, p, q);
        TreeNode right = help(root.right, p, q);
        if(left != null && right != null) return root;
        if(left != null) return left;
        if(right != null) return right;
        return null;
    }
}
```
**method 2:**

one pass, use a global variable to record the num of existing node.

ATTENTION!: **must do it in postOrder**
```java
class Solution {
    int count = 0;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode lca = help(root, p, q);
        return count == 2 ? lca : null;
        
    }
    private TreeNode help(TreeNode root, TreeNode p, TreeNode q){
        if(root == null) return root;
        TreeNode left = help(root.left, p, q);
        TreeNode right = help(root.right, p, q);
        if(root == p || root == q){
            count++;
            return root;
        }
        if(left != null && right != null) return root;
        if(left != null) return left;
        if(right != null) return right;
        return null;
    }
}
```
### [1650. Lowest Common Ancestor of a Binary Tree III](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iii/)

Given two nodes of a binary tree p and q, return their lowest common ancestor (LCA).

Each node will have a reference to its parent node. The definition for Node is below:

    class Node {
        public int val;
        public Node left;
        public Node right;
        public Node parent;
    }

method 1: as it provided us node.parent, so it would be easier, just use a set to record the one node's path from node to root, then to check whether they got lca.

```java
class Solution {
    public Node lowestCommonAncestor(Node p, Node q) {
        Set<Node> set = new HashSet<>();
        while(p != null){
            set.add(p);
            p = p.parent;
        }
        while(q != null){
            if(set.contains(q)){
                return q;
            }
            q = q.parent;
        }
        return null;
    }
}
```
method 2: linkedList qusetion's follow up, 大白话说就是从两个node一直往上找，到头了就换到另一个node继续往上找，如果有lca，总会相遇的。
```java
class Solution {
    public Node lowestCommonAncestor(Node p, Node q) {
        Node a = p;
        Node b = q;
        while(a != b){
            a = a == null ? q : a.parent;
            b = b == null ? p : b.parent;
         }
        return a;
    }
}
```
### [1676. Lowest Common Ancestor of a Binary Tree IV](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iv/)

same as above question, but we need to find a array of nodes' lca

use HashSet to store all the nodes, then do the same job...

```java
class Solution {
    Set<TreeNode> set = new HashSet<>();
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode[] nodes) {
        for(TreeNode node : nodes){
            set.add(node);
        }
        return help(root);
    }
    private TreeNode help(TreeNode root){
        if(root == null || set.contains(root)) return root;
        if(set.contains(root)) return root;
        TreeNode left = help(root.left);
        TreeNode right = help(root.right);
        if(right != null && left != null) return root;
        if(right != null) return right;
        if(left != null) return left;
        return null;
    }
}
```
### [1123. Lowest Common Ancestor of Deepest Leaves](https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/)

Given the root of a binary tree, return the lowest common ancestor of its deepest leaves.

Recall that:

- The node of a binary tree is a leaf if and only if it has no children
- The depth of the root of the tree is 0. if the depth of a node is d, the depth of each of its children is d + 1.
- The lowest common ancestor of a set S of nodes, is the node A with the largest depth such that every node in S is in the subtree with root A.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4]
Output: [2,7,4]
Explanation: We return the node with value 2, colored in yellow in the diagram.
The nodes coloured in blue are the deepest leaf-nodes of the tree.
Note that nodes 6, 0, and 8 are also leaf nodes, but the depth of them is 2, but the depth of nodes 7 and 4 is 3.
```

First, get the max depth of this binary tree's left child tree and right child tree, then make a comparison:

- left == root return root
- left > right, return recursion(root.left)
- left < right, return recursion(root.right)

```java
class Solution {
    public TreeNode lcaDeepestLeaves(TreeNode root) {
        int left = depth(root.left);
        int right = depth(root.right);
        if(left == right){
            return root;
        }
        if(left > right){
            return lcaDeepestLeaves(root.left);
        }
        return lcaDeepestLeaves(root.right);
    }
    private int depth(TreeNode node){
        if(node == null){
            return 0;
        }
        return 1 + Math.max(depth(node.left), depth(node.right));
    }
}
```

## 信息传递
### [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

    Input: root = [1,2,3,null,5]
    Output: ["1->2->5","1->3"]

```java
class Solution {
    List<String> res = new ArrayList<>();
    List<String> path = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        if(root == null) return res;
        dfs(root);
        return res;
    }
    private void dfs(TreeNode root){
        if(root == null){
            return;
        }
        path.add(String.valueOf(root.val));
        if(root.left == null && root.right == null){
            res.add(String.join("->", path));
        }
        dfs(root.left);
        dfs(root.right);
        path.remove(path.size() - 1); 
    }
}
```
### [1448. Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)

Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

 

**Example 1:**
![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)

    Input: root = [3,1,4,3,null,1,5]
    Output: 4
    Explanation: Nodes in blue are good.
    Root Node (3) is always a good node.
    Node 4 -> (3,4) is the maximum value in the path starting from the root.
    Node 5 -> (3,4,5) is the maximum value in the path
    Node 3 -> (3,1,3) is the maximum value in the path.

global varibale to store the res, pass the cur max value into recursion

```java
class Solution {
    int res = 0;
    public int goodNodes(TreeNode root) {
        dfs(root, Integer.MIN_VALUE);
        return res;
        
    }
    private void dfs(TreeNode root, int max){
        if(root == null) return;
        max = Math.max(max, root.val);
        if(root.val >= max) res++;
        dfs(root.left, max);
        dfs(root.right, max);
    }
}
```
### [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

1. if left or right < 0, we had rather pass 0 than negative value
2. record every recursion's max res
3. in the end of one recursion, we should return `root.val + Math.max(left, right)`
```java
class Solution {
    int res = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        help(root);
        return res;
        
    }
    private int help(TreeNode root){
        if(root == null) return 0;
        int left = Math.max(0, help(root.left));
        int right = Math.max(0, help(root.right));
        res = Math.max(res, left + right + root.val);
        return Math.max(left, right) + root.val;
    }
}
```
### [1120. Maximum Average Subtree](https://leetcode.com/problems/maximum-average-subtree/)

use array to store total value and root num;

```java
class Solution {
    double max = Double.MIN_VALUE;
    public double maximumAverageSubtree(TreeNode root) {
        dfs(root);
        return max;
        
    }
    private int[] dfs(TreeNode root){
        if(root == null) return new int[2];
        int[] res = new int[2];
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);
        res[0] = left[0] + right[0] + root.val;
        res[1] = left[1] + right[1] + 1;
        max = Math.max(max, (double)res[0] / (double)res[1]);
        return res;
        
    }
}
```
### [1372. Longest ZigZag Path in a Binary Tree](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/)

You are given the root of a binary tree.

A ZigZag path for a binary tree is defined as follow:

- Choose any node in the binary tree and a direction (right or left).
- If the current direction is right, move to the right child of the current node; otherwise, move to the left child.
- Change the direction from right to left or from left to right.
- Repeat the second and third steps until you can't move in the tree.

Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return the longest ZigZag path contained in that tree.

 
**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png)

    Input: root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1,null,1]
    Output: 3
    Explanation: Longest ZigZag path in blue nodes (right -> left -> right).



```java
class Solution {
    int res = Integer.MIN_VALUE;
    public int longestZigZag(TreeNode root) {
        dfs(root);
        return res == 0 ? 0 : res - 1;
    }
    private int[] dfs(TreeNode root){
        int[] cur = new int[2];
        if(root == null) return cur;
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);
        cur[0] = left[1] + 1;
        cur[1] = right[0] + 1;
        res = Math.max(res, Math.max(cur[0], cur[1]));
        return cur;
    }
}
```
### [549. Binary Tree Longest Consecutive Sequence II](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii/)

Given the root of a binary tree, return the length of the longest consecutive path in the tree.

This path can be either increasing or decreasing.

- For example, [1,2,3,4] and [4,3,2,1] are both considered valid, but the path [1,2,4,3] is not valid.


On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

 

Example 1:
![](https://assets.leetcode.com/uploads/2021/03/14/consec2-1-tree.jpg)

    Input: root = [1,2,3]
    Output: 2
    Explanation: The longest consecutive path is [1, 2] or [2, 1].

array to store root.left and right info

root.val == root.left + 1 -> indcreasing -> res[0] = left[0] + 1
root.val == root.left - 1 -> decreasing -> res[1] = left[1] + 1
root.val == root.right + 1 -> indcreasing -> res[0] = right[0] + 1
root.val == root.right - 1 -> decreasing -> res[1] = right[1] + 1


```java
class Solution {
    int max = 0;
    public int longestConsecutive(TreeNode root) {
        dfs(root);
        return max;
        
    }
    private int[] dfs(TreeNode root){
        int[] res = new int[2];
        if(root == null) return res;
        
        res[0] = 1;
        res[1] = 1;
        
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);
        
        if(root.left != null){
            if(root.val == root.left.val + 1){
                res[0] = left[0] + 1;
            }else if(root.val == root.left.val - 1){
                res[1] = left[1] + 1;
            }
        }
        if(root.right != null){
            if(root.val == root.right.val + 1){
                res[0] = Math.max(res[0], right[0] + 1);
            }else if(root.val == root.right.val - 1){
                res[1] = Math.max(res[1], right[1] + 1);
            }
        }
        max = Math.max(max, res[0] + res[1] - 1);
        return res;     
    }
}
```
## [938. Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/)

Given the root `node` of a binary search tree and two integers `low` and `high`, return the sum of values of all nodes with a value in the inclusive range `[low, high]`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/bst1.jpg)

Input: root = [10,5,15,3,7,null,18], low = 7, high = 15
Output: 32
Explanation: Nodes 7, 10, and 15 are in the range [7, 15]. 7 + 10 + 15 = 32.
dfs，二叉搜索树的普通解法，看代码即可：

```java
class Solution {
    int res = 0;
    public int rangeSumBST(TreeNode root, int low, int high) {
        dfs(root, low, high);
        return res;
    
    }
    private void dfs(TreeNode node, int low, int high){
        if(node == null){
            return;
        }
        if(node.val >= low && node.val <= high){
            res += node.val;
        }
        dfs(node.left, low, high);
        dfs(node.right, low, high);

    }
}
```

## 1305 All Elements in Two Binary Search Trees

Given two binary search trees root1 and root2.

Return a list containing all the integers from both trees sorted in ascending order.

**Example 1:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200905194207.png)

```
Input: root1 = [2,1,4], root2 = [1,0,3]
Output: [0,1,1,2,3,4]
```

**Example 2:**

```
Input: root1 = [0,-10,10], root2 = [5,1,7,0,2]
Output: [-10,0,0,1,2,5,7,10]
```

**Example 3:**

```
Input: root1 = [], root2 = [5,1,7,0,2]
Output: [0,1,2,5,7]
```

**Example 4:**

```
Input: root1 = [0,-10,10], root2 = []
Output: [-10,0,10]
```

**Example 5:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200905194218.png)

```
Input: root1 = [1,null,8], root2 = [8,1]
Output: [1,1,8,8]
```

普通方法，就是把两个 BST 中的元素遍历到一个 List 中，最后利用 `Collections.sort()`对 List 进行排序操作，但是这个方法并没有利用 BST 的优势

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

方法二，通过中序遍历，利用 BST 的特性，这样保存到每一个 List 中的元素都是有序的，最后我们再利用 Merge Sort 即可节省大量的时间

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


## 110. Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

**Example 1:**

Given the following tree `[3,9,20,null,null,15,7]`:

```
3
     / \
    9  20
      /  \
     15   7
```

Return true.

**Example 2:**

Given the following tree `[1,2,2,3,3,null,null,4,4]`:

```
1
     / \
    2   2
   / \
  3   3
 / \
4   4
```

思路：[Leetcode 有一篇文章写的很不错](https://leetcode-cn.com/problems/balanced-binary-tree/solution/ping-heng-er-cha-shu-by-leetcode-solution/)，这个题可以有两个思路，一个是**自顶向下**，一个是**自底向上**

**自顶向下的递归：**

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        } else {
            return Math.abs(height(root.left) - height(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
        }
    }

    public int height(TreeNode root) {
        if (root == null) {
            return 0;
        } else {
            return Math.max(height(root.left), height(root.right)) + 1;
        }
    }
}
```

方法一由于是自顶向下递归，因此对于同一个节点，函数 \texttt{height}height 会被重复调用，导致时间复杂度较高。如果使用自底向上的做法，则对于每个节点，函数 \texttt{height}height 只会被调用一次。

自底向上递归的做法类似于后序遍历，对于当前遍历到的节点，先递归地判断其左右子树是否平衡，再判断以当前节点为根的子树是否平衡。如果一棵子树是平衡的，则返回其高度（高度一定是非负整数），否则返回 -1−1。如果存在一棵子树不平衡，则整个二叉树一定不平衡。

**自底向上的递归：**

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) >= 0;
    }

    public int height(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftHeight = height(root.left);
        int rightHeight = height(root.right);
        if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        } else {
            return Math.max(leftHeight, rightHeight) + 1;
        }
    }
}
```

## 95 Unique Binary Search Trees II

Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.

**Example:**

```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if(n == 0) return new ArrayList<>();
        return helper(1, n);
    }
  
    public List<TreeNode> helper(int begin, int end){
        List<TreeNode> res = new ArrayList<>();
  
        if(begin > end){
            res.add(null);
            return res;
        }
  
        for(int i = begin; i <= end; i++){
            List<TreeNode> left = helper(begin, i - 1);
            List<TreeNode> right = helper(i + 1, end);
  
            for(TreeNode l : left){
                for(TreeNode r : right){
                    TreeNode cur = new TreeNode(i);
                    cur.left = l;
                    cur.right = r;
                    res.add(cur);
                }
            }
        }
        return res;
    }
}
```

## 96 Unique Binary Search Trees

Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

**Example:**

```
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2S
    /     /       \                 \
   2     1         2                 3
```

思路：这其实是一个动态规划的题目，

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
  
        dp[0] = 1;
        dp[1] = 1;
  
        for(int i = 2; i <= n; i++){
            for(int j = 1; j <= i; j++){
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
}
```

## 98 Validate Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

**Example 1:**

```
2
 / \
1   3

Input: [2,1,3]
Output: true
```

**Example 2:**

```
5
/ \
1   4
   / \
  3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

要解决这道题首先我们要了解二叉搜索树有什么性质可以给我们利用，由题目给出的信息我们可以知道：**如果该二叉树的左子树不为空，则左子树上所有节点的值均小于它的根节点的值； 若它的右子树不空，则右子树上所有节点的值均大于它的根节点的值；它的左右子树也为二叉搜索树。**

这启示我们设计一个递归函数 `helper(root, min, max)` 来递归判断，函数表示考虑以 `root` 为根的子树，判断子树中所有节点的值是否都在 `(l,r)(l,r)` 的范围内（注意是开区间）。如果 `root` 节点的值 `val` 不在 `(l,r)(l,r)` 的范围内说明不满足条件直接返回，否则我们要继续递归调用检查它的左右子树是否满足，如果都满足才说明这是一棵二叉搜索树。

那么根据二叉搜索树的性质，在递归调用左子树时，我们需要把上界 `max` 改为 `root`，即调用 `helper(root.left, min, root)`，因为左子树里所有节点的值均小于它的根节点的值。同理递归调用右子树时，我们需要把下界 `lower` 改为 `root`，即调用 `helper(root.right, root, max)`。

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return helper(root, null, null);
    }
  
    public boolean helper(TreeNode root, TreeNode min, TreeNode max){
        if(root == null) return true;
  
        if(min != null && root.val <= min.val) return false;
        if(max != null && root.val >= max.val) return false;
  
        return helper(root.left, min, root) && helper(root.right, root, max);
    }
}
```

这个题还有一个中序遍历的方法，基于方法一中提及的性质，我们可以进一步知道二叉搜索树「中序遍历」得到的值构成的序列一定是升序的，这启示我们在中序遍历的时候实时检查当前节点的值是否大于前一个中序遍历到的节点的值即可。如果均大于说明这个序列是升序的，整棵树是二叉搜索树，否则不是，下面的代码我们使用栈来模拟中序遍历的过程。

可能由读者不知道中序遍历是什么，我们这里简单提及一下，中序遍历是二叉树的一种遍历方式，它先遍历左子树，再遍历根节点，最后遍历右子树。而我们二叉搜索树保证了左子树的节点的值均小于根节点的值，根节点的值均小于右子树的值，因此中序遍历以后得到的序列一定是升序序列。

```java
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        // 访问左子树
        if (!isValidBST(root.left)) {
            return false;
        }
        // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足 BST，返回 false；否则继续遍历。
        if (root.val <= pre) {
            return false;
        }
        pre = root.val;
        // 访问右子树
        return isValidBST(root.right);
    }
}
```

## 113 Path Sum II

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

Note: A leaf is a node with no children.

**Example:**

Given the below binary tree and sum = 22,

```
5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
```

**Return:**

```
[
    [5,4,11,2],
    [5,8,4,5]
]
```

思路：
标准的回溯解题思路

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
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> res = new ArrayList<>();
        Deque<Integer> path = new ArrayDeque<>();
  
        if(root == null){
            return res;
        }
  
        dfs(root, res, path, sum);
        return res;
  
    }
    public void dfs(TreeNode root, List<List<Integer>> res, Deque<Integer> path, int sum){
        if(root == null){
            return;
        }
        path.addLast(root.val);
        if(root.val == sum && root.left == null && root.right == null){
            res.add(new ArrayList<>(path));
        }
        dfs(root.left, res, path, sum - root.val);
        dfs(root.right, res, path, sum - root.val);
        path.removeLast();
    }
}
```

## 437 Path Sum III

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

**Example:**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

马克弟弟有话说：这个题微信老哥给的解释十分清晰：

总结：
这道题目引入了一个点：前缀和，就是到达当前元素的路径上，之前所有元素的和。

那么在写代码的时候，如何表示前缀和？
从 root 开始往下 dfs, 记录下所到的每一个节点的前缀和

比如：存储当前节点 i 路径下的所有父节点 j 到 root 的和。那么 `sum[i,j] = sum[i] - sum[j]`

如果你看不太懂，那个一步到位使用 hashmap 的方法，那么咱们一步一步的拆解。

咱们抛开链接的题解，先用最朴素的方法来做，代码如下

```java
int counter = 0;
private void helper(TreeNode node, int[] sums, int level, int target){
    // 先检查本节点是否存在于上面节点的和==target 的情况
    if(sums[level] == target) ++counter;
    for(int i = 0; i < level; ++i){
        // System.out.println(i + "\t" + (sums[level] - sums[i]));
        if(sums[level] - sums[i] == target) ++counter;
    }
    // sum 存储从根节点到本节点，每个节点的到 root 的和
    if(node.left != null){
        sums[level + 1] = sums[level] + node.left.val;
        helper(node.left, sums, level + 1, target);
        sums[level + 1] = 0;
    }
    if(node.right != null){
        sums[level + 1] = sums[level] + node.right.val;
        helper(node.right, sums, level + 1, target);
        sums[level + 1] = 0;
    }
}
public int pathSum(TreeNode root, int target) {
    if(root == null) return 0;
    int[] sums = new int[1001];
    sums[0] = root.val;
    helper(root, sums, 0, target );
    return counter;
}
```

完事了，我们考虑优化：其实所有的和可以用 hash 表存取，更快。
所以，优化之后的代码，如下：

```java
int counter = 0;
  private void helper(TreeNode node, HashMap<Integer, Integer> sums, int lastSum, int target){
      // 检查与根节点
      int thisSum = lastSum + node.val;
      if(thisSum == target) ++counter;
      //检查与其它节点
      if(sums.containsKey(thisSum - target)) {
          counter += sums.get(thisSum - target);
      }
      //将本节点加入
      if(sums.containsKey(thisSum)){
          sums.replace(thisSum, sums.get(thisSum) + 1);
      }
      else sums.put(thisSum, 1);
      if(node.left != null){
          helper(node.left, sums,thisSum, target);
      }
      if(node.right != null){
          helper(node.right, sums, thisSum, target);
      }
      //将本节点挪出
      if(sums.get(thisSum) > 1) sums.replace(thisSum, sums.get(thisSum) - 1);
      else sums.remove(thisSum);
  }
  public int pathSum(TreeNode root, int target) {
      if(root == null) return 0;
      HashMap<Integer, Integer> sums = new HashMap<>();
      helper(root, sums, 0, target );
      return counter;
  }
```

其实，这道题之所以困惑，主要是有
（1）之前没有接触过前缀和的概念
（2）题解直接用了 hashmap, 缺少最朴素的解法

所以，先用“ 笨” 办法写出来，再考虑优化

---

这道题用到了一个概念，叫前缀和。就是到达当前元素的路径上，之前所有元素的和。

前缀和怎么应用呢？

如果两个数的**前缀总和**是相同的，那么这些**节点之间的元素总和为零**。进一步扩展相同的想法，如果前缀总和 currSum，在节点 A 和节点 B 处相差 target，则位于节点 A 和节点 B 之间的元素之和是 target。

因为本题中的路径是一棵树，从根往任一节点的路径上（不走回头路），**有且仅有一条路径**，因为**不存在环**。（如果存在环，前缀和就不能用了，需要改造算法）

抵达当前节点（即 B 节点）后，将前缀和累加，然后查找在前缀和上，有没有**前缀和 currSum-target 的节点**（即 A 节点），存在即表示从 A 到 B 有一条路径之和满足条件的情况。结果加上满足**前缀和 currSum-target 的节点**的数量。然后递归进入左右子树。

左右子树遍历完成之后，回到当前层，需要把当前节点添加的前缀和去除。避免回溯之后影响上一层。因为思想是前缀和，不属于前缀的，我们就要去掉它。

时间复杂度：每个节点只遍历一次，O(N).

空间复杂度：开辟了一个 hashMap,O(N).

核心代码

```java
// 当前路径上的和
currSum += node.val;
// currSum-target 相当于找路径的起点，起点的 sum+target=currSum，当前点到起点的距离就是 target
res += prefixSumCount.getOrDefault(currSum - target, 0);
// 更新路径上当前节点前缀和的个数
prefixSumCount.put(currSum, prefixSumCount.getOrDefault(currSum, 0) + 1);
```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int pathSum(TreeNode root, int sum) {
        // key 是前缀和，value 是大小为 key 的前缀和出现的次数
        Map<Integer, Integer> prefixSumCount = new HashMap<>();
        // 前缀和为 0 的一条路径
        prefixSumCount.put(0, 1);
        // 前缀和的递归回溯思路
        return recursionPathSum(root, prefixSumCount, sum, 0);
    }

    /**
     * 前缀和的递归回溯思路
     * 从当前节点反推到根节点（反推比较好理解，正向其实也只有一条），有且仅有一条路径，因为这是一棵树
     * 如果此前有和为 currSum-target, 而当前的和又为 currSum, 两者的差就肯定为 target 了
     * 所以前缀和对于当前路径来说是唯一的，当前记录的前缀和，在回溯结束，回到本层时去除，保证其不影响其他分支的结果
     * @param node 树节点
     * @param prefixSumCount 前缀和 Map
     * @param target 目标值
     * @param currSum 当前路径和
     * @return 满足题意的解
     */
    private int recursionPathSum(TreeNode node, Map<Integer, Integer> prefixSumCount, int target, int currSum) {
        // 1. 递归终止条件
        if (node == null) {
            return 0;
        }
        // 2. 本层要做的事情
        int res = 0;
        // 当前路径上的和
        currSum += node.val;

        //---核心代码
        // 看看 root 到当前节点这条路上是否存在节点前缀和加 target 为 currSum 的路径
        // 当前节点->root 节点反推，有且仅有一条路径，如果此前有和为 currSum-target, 而当前的和又为 currSum, 两者的差就肯定为 target 了
        // currSum-target 相当于找路径的起点，起点的 sum+target=currSum，当前点到起点的距离就是 target
        res += prefixSumCount.getOrDefault(currSum - target, 0);
        // 更新路径上当前节点前缀和的个数
        prefixSumCount.put(currSum, prefixSumCount.getOrDefault(currSum, 0) + 1);
        //---核心代码

        // 3. 进入下一层
        res += recursionPathSum(node.left, prefixSumCount, target, currSum);
        res += recursionPathSum(node.right, prefixSumCount, target, currSum);

        // 4. 回到本层，恢复状态，去除当前节点的前缀和数量
        prefixSumCount.put(currSum, prefixSumCount.get(currSum) - 1);
        return res;
    }
}
```

## 250 Count Univalue Subtrees

Given the root of a binary tree, return the number of uni-value subtrees.

A uni-value subtree means all nodes of the subtree have the same value.

**Example 1:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201123191313.png)

```
Input: root = [5,1,5,5,5,null,5]
Output: 4
```

**Example 2:**

```
Input: root = []
Output: 0
```

**Example 3:**

```
Input: root = [5,5,5,5,5,null,5]
Output: 6
```

思路：

节点 node 若是同值子树点，则其左右子树首先都是同值子树点，并且左右孩子的 val 与 node 的 val 相同。介于此，遍历 node 的时候，对左右子树 dfs 返回一个 bool 值，若都为真，再将三者的 val 进行对比，否则直接返回 false。

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
    int res;
    public int countUnivalSubtrees(TreeNode root) {
        helper(root);
        return res;
    }
  
    public boolean helper(TreeNode root){
        if(root == null){
            return true;
        }
  
        //递归查看节点左边是不是满足同值子树
        boolean left = helper(root.left);
        //递归查看节点右边是不是满足同值子树
        boolean right = helper(root.right);
        //如果都是的话，在判断当前节点是不是满足同值子树
        if(left && right){
            //这里有两种情况
            //1. 左子树为空 || 根结点值等于根的左子树的值
            //2. 右子树为空 || 根结点值等于根的右子树的值
            if(root.left != null && root.val != root.left.val){
                return false;
            }
            if(root.right != null && root.val != root.right.val){
                return false;
            }
            res++;
            return true;
        }
        return false;
    }
}
```

# Sweep Line

## 391. Number of Airplanes in the Sky (LintCode)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210721164125.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210721164301.png)

碰到interval的start，也就是起飞一架飞机，当前天上的飞机数++。
碰到interval的end，也就是降落一架飞机，当前天上的飞机数--。
我们分别把所有的start和所有的end放进两个数组，并排序。
然后从第一个start开始统计，碰到start就加一，碰到end就减一。并且同时维护一个最大飞机数的max。
注意图上在4这个位置，有一上一下。题目说先算降落的再算起飞的。
当所有的start统计过以后，我们就有答案了。后面的end只会往下减，所以不用再看了。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210721164348.png)

## [252. Meeting Rooms](https://leetcode.com/problems/meeting-rooms/)

Given an array of meeting time intervals where intervals[i] = [starti, endi], determine if a person could attend all meetings.

**Example 1:**

    Input: intervals = [[0,30],[5,10],[15,20]]
    Output: false

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        for(int i = 1; i < intervals.length; i++){
            if(intervals[i][0] < intervals[i - 1][1]){
                return false;
            }
        }
        return true;
    }
}
```
## [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)
Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.

**Example 1:**

    Input: intervals = [[0,30],[5,10],[15,20]]
    Output: 2

method 1: 
count plane
```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        List<int[]> list = new ArrayList<>();
        for(int[] i : intervals){
            list.add(new int[]{i[0], 1});
            list.add(new int[]{i[1], -1});
        }
        
        Collections.sort(list, (a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
        int count = 0;
        int res = 0;
        for(int[] cur : list){
            count += cur[1];
            res = Math.max(res, count);
        }
        return res;
    }
}
```
method 2:
priotriyQueue
```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        if(intervals != null) pq.offer(intervals[0]);
        for(int i = 1; i < intervals.length; i++){
            int[] cur = pq.poll();
            if(intervals[i][0] >= cur[1]){
                pq.offer(intervals[i]);
                
            }else{
                pq.offer(intervals[i]);
                pq.offer(cur);
            }
        }
        return pq.size();
    }
}
```
## [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> res = new ArrayList<>();
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        for(int[] cur : intervals){
            int size = res.size();
            if(size == 0 || res.get(size - 1)[1] < cur[0]){
                res.add(cur);
            }else{
                res.get(size - 1)[1] = Math.max(cur[1], res.get(size - 1)[1]);
            }
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```
## [57. Insert Interval](https://leetcode.com/problems/insert-interval/)
Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

**Example 1:**

    Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
    Output: [[1,5],[6,9]]

**Example 2:**

    Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
    Output: [[1,2],[3,10],[12,16]]
    Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

just iterate the intervals and compare the `newInterval[0]` with `interval[i][1]`,
if `intervals[i][1] < newInterval[0]`, just add intervals[i] into res array,
if `intervals[i][0] <= newInterval[1]`, which means inetrvals[i] has the intersection with newInterval, see if `intervals[i][0] > newInterval[1]`, it would be `---- **** ----`
after that, almost done, then just put the rest intervals into res array.

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        
        List<int[]> res = new ArrayList<>();
        int i = 0;
        int n = intervals.length;
        while(i < n && intervals[i][1] < newInterval[0]){
            res.add(intervals[i]);
            i++;
        }
        while(i < n && intervals[i][0] <= newInterval[1]){
            newInterval[0] = Math.min(intervals[i][0], newInterval[0]);
            newInterval[1] = Math.max(intervals[i][1], newInterval[1]);
            i++;
        }
        res.add(newInterval);
        while(i < n){
            res.add(intervals[i]);
            i++;
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```
## [1272. Remove Interval](https://leetcode.com/problems/remove-interval/)
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210725184606.png)

```java
class Solution {
    public List<List<Integer>> removeInterval(int[][] intervals, int[] toBeRemoved) {
        List<List<Integer>> res = new ArrayList<>();
        for(int[] cur : intervals){
            if(cur[1] <= toBeRemoved[0] || cur[0] >= toBeRemoved[1]){
                res.add(Arrays.asList(cur[0], cur[1]));
            }else{
                if(cur[0] < toBeRemoved[0]){
                    res.add(Arrays.asList(cur[0], toBeRemoved[0]));
                }
                if(cur[1] > toBeRemoved[1]){
                    res.add(Arrays.asList(toBeRemoved[1], cur[1]));
                }
            }
        }
        return res;
    }
}
```
## [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

**Example 1:**

    Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
    Output: 1
    Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.

use a variable end to record last value which is less or equal than cur[0], if so, update end, if not, res++.

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
        int res = 0;
        int end = Integer.MIN_VALUE;
        for(int[] cur : intervals){
            if(end <= cur[0]){
                end = cur[1];
            }else{
                res++;
            }
        }
        return res;
    }
}   
```
## [1288. Remove Covered Intervals](https://leetcode.com/problems/remove-covered-intervals/)

Given a list of `intervals`, remove all intervals that are covered by another interval in the list.

Interval `[a,b)` is covered by interval `[c,d)` if and only if `c <= a` and `b <= d`.

After doing so, return the number of remaining intervals.

 

**Example 1:**

    Input: intervals = [[1,4],[3,6],[2,8]]
    Output: 2
    Explanation: Interval [3,6] is covered by [2,8], therefore it is removed.

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210725231848.png)

sort `interval[0]` in increasing order, sort `interval[1]` in descending order.

use end to represent last `interval[1]`, so just need to focus on the end and `cur[1]`.

if `end < cur[1]`, which means not cover, res++, then update end.

```java
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int end = Integer.MIN_VALUE;
        int res = 0;
        for(int[] cur : intervals){
            if(end < cur[1]){
                res++;
                end = cur[1];
            }
        }
        return res;
    }
}
```
## [352. Data Stream as Disjoint Intervals](https://leetcode.com/problems/data-stream-as-disjoint-intervals/)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726120522.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726120556.png)

need to master some APIs of **TreeSet: lower, higher**
```java
  // 返回Set的第一个元素
    public E first() {
        return m.firstKey();
    }

    // 返回Set的最后一个元素
    public E last() {
        return m.lastKey();
    }

    // 返回Set中小于e的最大元素
    public E lower(E e) {
        return m.lowerKey(e);
    }

    // 返回Set中小于/等于e的最大元素
    public E floor(E e) {
        return m.floorKey(e);
    }

    // 返回Set中大于/等于e的最小元素
    public E ceiling(E e) {
        return m.ceilingKey(e);
    }

    // 返回Set中大于e的最小元素
    public E higher(E e) {
        return m.higherKey(e);
    }

    // 获取第一个元素，并将该元素从TreeMap中删除。
    public E pollFirst() {
        Map.Entry<E,?> e = m.pollFirstEntry();
        return (e == null)? null : e.getKey();
    }

    // 获取最后一个元素，并将该元素从TreeMap中删除。
    public E pollLast() {
        Map.Entry<E,?> e = m.pollLastEntry();
        return (e == null)? null : e.getKey();
    }
```

```java
class SummaryRanges {
    TreeSet<int[]> set;

    /** Initialize your data structure here. */
    public SummaryRanges() {
        set = new TreeSet<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
    }
    
    public void addNum(int val) {
        int[] interval = new int[]{val, val};
        if(set.contains(interval)) return;
        int[] low = set.lower(interval), high = set.higher(interval);
        if(high != null && high[0] == val) return;
        if(low != null && low[1] + 1 == val && high != null && val + 1 == high[0]){
            low[1] = high[1];
            set.remove(high);
        }else if(low != null && low[1] + 1 >= val){
            low[1] = Math.max(low[1], val);
        }else if(high != null && val + 1 == high[0]){
            high[0] = val;
        }else{
            set.add(interval);
        }
    }
    
    public int[][] getIntervals() {
        List<int[]> res = new ArrayList<>();
        for(int[] cur : set){
            res.add(cur);
        }
        return res.toArray(new int[set.size()][2]);
    }
}

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges obj = new SummaryRanges();
 * obj.addNum(val);
 * int[][] param_2 = obj.getIntervals();
 */
```
## [1229. Meeting Scheduler](https://leetcode.com/problems/meeting-scheduler/)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726123248.png)

- sort each slot's start time in increasing order
- two pointer to iterate
- get the max start time and min end time
- then determine whether end - start >= duration
- if not, **greedy**, move the pointer that ends first



```java
class Solution {
    public List<Integer> minAvailableDuration(int[][] slots1, int[][] slots2, int duration) {
        Arrays.sort(slots1, (a, b) -> a[0] - b[0]);
        Arrays.sort(slots2, (a, b) -> a[0] - b[0]);
        int index1 = 0;
        int index2 = 0;
        while(index1 < slots1.length && index2 < slots2.length){
            int start = Math.max(slots1[index1][0], slots2[index2][0]);
            int end = Math.min(slots1[index1][1], slots2[index2][1]);
            if(end - start >= duration){
                return List.of(start, start + duration);
            }else if(slots1[index1][1] < slots2[index2][1]){
                index1++;
            }else{
                index2++;
            }
        }
        return new ArrayList<>();
    }
}


```
## [986. Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726124200.png)

- **same with last one(1229. Meeting Scheduler)**
- just in this question, we don't need to determine the duration

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        List<int[]> res = new ArrayList<>();
        int index1 = 0, index2 = 0;
        while(index1 < firstList.length && index2 < secondList.length){
            int start = Math.max(firstList[index1][0], secondList[index2][0]);
            int end = Math.min(firstList[index1][1], secondList[index2][1]);
            if(end >= start){
                res.add(new int[]{start, end});
            }
            if(firstList[index1][1] < secondList[index2][1]){
                index1++;
            }else{
                index2++;
            }
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```

## [759. Employee Free Time](https://leetcode.com/problems/employee-free-time/)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726124625.png)

- put all the time intervals into PriorityQueue, sort a.start in increasing order
- if cur.end > next.start, which means they got intersection, merge them.
- if not, add to res array(cur.end, next.start), update cur 

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726125902.png)

```java
class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<Interval> res = new ArrayList<>();
        
        PriorityQueue<Interval> pq = new PriorityQueue<>((a, b) -> a.start - b.start);
        for(List<Interval> list : schedule){
            for(Interval interval : list){
                pq.add(interval);
            }
        }
        Interval cur = pq.poll();
        while(!pq.isEmpty()){
            if(cur.end >= pq.peek().start){
                cur.end = Math.max(cur.end, pq.poll().end);
            }else{
                res.add(new Interval(cur.end, pq.peek().start));
                cur = pq.poll();
            }
        }
        return res;
    }
}
```

## [218. The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726130347.png)

- hard, high freq
- first, reconstruct the height array, we use 2d array to represent the height, just like count plane, for example: `[2, 9, 10]` -> `[2, 9] & [2, -9]`. Try to convert this question to another scenario: 
  - `height[0]` -> take off
  - `height[1]` -> land
  - `height[2]` -> height change.
- then sort the height list: `Collections.sort(height, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0])` 
  - why `b[1] - a[1]`? cuz take off is postive, landing is negetive, but the take off ahead of landing 
- use PriorityQueue to record current largest height
- iterate height list, if `height[1] > 0`, which means take off, add to queue, if < 0, which means landing, remove from queue.
- get current largest height from pq, if curMax != preMax, means largest height got changed, that is what we want, so add into res list
- update preMax
- pq.remove()时间复杂度：search是O(n)删是logn 随机删= search + delete

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20210726171053.png)

```java
class Solution {
    public List<List<Integer>> getSkyline(int[][] buildings) {
        List<List<Integer>> res = new ArrayList<>();
        List<int[]> height = new ArrayList<>();
        for(int[] cur : buildings){
            height.add(new int[]{cur[0], cur[2]});
            height.add(new int[]{cur[1], -cur[2]});
        }
        
        Collections.sort(height, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        pq.offer(0);
        int preMax = 0;
        for(int[] h : height){
            if(h[1] > 0) pq.offer(h[1]);
            else pq.remove(-h[1]);
            int curMax = pq.peek();
            if(curMax != preMax){
                res.add(List.of(h[0], curMax));
                preMax = curMax;
            }
        }
        return res;
    }
}
```

# Array

## N sum Question

### [1. Two Sum](https://leetcode.com/problems/two-sum/)

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

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

### [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)

Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example 1:**

Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
思路就是双指针，从头扫到尾，在这个过程中创建两个指针，`left = i + 1`, `right = nums.length - 1`。
然后求 `sum = nums[i] + nums[left] + nums[right]`，最后判断和 target 的接近程度，不断刷新 res 的值。

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        // 注意这里，我本来初始化 res 是 int 最大值，但是在下面 sum - target 的过程中，target 有可能是负数，就导致整形溢出。这里要考虑一下。
        int res = Integer.MAX_VALUE / 2;
        for(int i = 0; i < nums.length; i++){
            int left = i + 1;
            int right = nums.length - 1;
            while(left < right){
                int sum  = nums[i] + nums[left] + nums[right];
                if(Math.abs(sum - target) < Math.abs(res - target)){
                    res = sum;
                }
                if(sum > target){
                    right--;
                }else if(sum < target){
                    left++;
                }else{
                    return res;
                }
            }
        }
        return res;
    }
}
```

## Interval Question (insert, merge)

### 252. [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/)

Given an array of meeting time intervals where intervals[i] = [starti, endi], determine if a person could attend all meetings.

**Example 1:**

Input: intervals = [[0,30],[5,10],[15,20]]
Output: false
**Example 2:**

Input: intervals = [[7,10],[2,4]]
Output: true
思路很简单，看代码即可。

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        for(int i = 1; i < intervals.length; i++){
            if(intervals[i][0] < intervals[i - 1][1]){
                return false;
            }
        }
        return true;
    }
}
```

### [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Example 1:**

Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
**Example 2:**

Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
首先重写 `Arrays.sort`，根据子数组的第一位，对原数组进行重新 sort。

```
1                2             3
```

---

```
-------        -----                ------
```

一共有以上三种情况，其中 1 和 2 可以归在一类里面考虑，我们只需要比较 res 数组中最后一个的第二位 和 待处理数组的第一位的大小，如果待处理的第一位比 res 数组最后一个的第二位大，那么直接加进去就好。如果小，那么接着比较各自的第二位即可。

要注意：List 转数组用 toArray() 的正确写法

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        int n = intervals.length;
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        List<int[]> res = new ArrayList<>();
        for(int i = 0; i < n; i++){
            int L = intervals[i][0];
            int R = intervals[i][1];
            if(res.size() == 0 || res.get(res.size() - 1)[1] < L){
                res.add(intervals[i]);
            }else{
                res.get(res.size() - 1)[1] = Math.max(R, res.get(res.size() - 1)[1]);
            }
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```

### [57. Insert Interval](https://leetcode.com/problems/insert-interval/)

Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

**Example 1:**

Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
**Example 2:**

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        int n = intervals.length;
        List<int[]> res = new ArrayList<>();
        int i = 0;
        while(i < n && intervals[i][1] < newInterval[0]){
            res.add(intervals[i]);
            i++;
        }
        int[] tmp = new int[]{newInterval[0], newInterval[1]};
        while(i < n && intervals[i][0] <= newInterval[1]){
            tmp[0] = Math.min(intervals[i][0], tmp[0]);
            tmp[1] = Math.max(intervals[i][1], tmp[1]);
            i++;
        }
        res.add(tmp);
        while(i < n){
            res.add(intervals[i]);
            i++;
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```

# Stack

## [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

You are given a string s consisting of lowercase English letters. A duplicate removal consists of choosing two adjacent and equal letters and removing them.

We repeatedly make duplicate removals on s until we no longer can.

Return the final string after all such duplicate removals have been made. It can be proven that the answer is unique.

**Example 1:**

```
Input: s = "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
```

classic question of using stack to solve! But in this question, we could just use StringBuilder instead

```java
class Solution {
    public String removeDuplicates(String s) {
        StringBuilder sb = new StringBuilder();
        int cnt = -1;
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(cnt > -1 && sb.charAt(cnt) == c){
                sb.deleteCharAt(cnt);
                cnt--;
            }else{
                sb.append(c);
                cnt++;
            }
        }
        return sb.toString();
    }
}
```

# Greedy

## [135. Candy](https://leetcode.com/problems/candy/)

There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute the candies to the children.

**Example 1:**

```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```

Greedy, suppose 2 students (A, B) are next to each other, A is on the left, B is on the right.

so need to meet 2 condition:

1. left rule: ratingB > ratingA, candyB > candyA
2. right rule: ratingA > ratingB, candyA > candyB

init res array and assign each child one candy, the iterate rating array, if ratingB > ratingA, candyB = candyA + 1, the same rule for other side.

```java
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, 1);
        Arrays.fill(right, 1);
        for(int i = 1; i < n; i++){
            if(ratings[i] > ratings[i - 1]){
                left[i] = left[i - 1] + 1;
            }
        }
        for(int i = n - 2; i >= 0; i--){
            if(ratings[i] > ratings[i + 1]){
                right[i] = right[i + 1] + 1;
            }
        }
        int res = 0;
        for(int i = 0; i < n; i++){
            res += Math.max(left[i], right[i]);
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

## Maximum Subarray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**

```
Input: [-2,1,-3,4,-1,2,1,-5,4],  
Output: 6  
Explanation: [4,-1,2,1] has the largest sum = 6.
```

这个题的思路是：从头开始计算，一旦出现子序列和小于 0 的情况，就重新开始 (sum=0)，因为前面的和为负数的话，后面与它相加会变小。

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

```
Input: ["flower","flow","flight"]  
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]  
Output: ""
```

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

```
Input: 10  
Output: 4
```

Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

思路一：最容易想到的方法就是写两个 for，让该数字从 1 开始除，如果可以除，那么就是素数。这个方法当面临一个特别大的数的时候，就会很耗费时间。所以不推荐

```java

```

思路二：利用数学方法：**埃拉托斯特尼筛法**

![](https://images2017.cnblogs.com/blog/1157228/201709/1157228-20170907193558741-1720107409.gif)

首先，将 2 到 n 范围内的所有整数写下来。其中最小的数字 2 是素数。将表中所有 2 的倍数都划去。表中剩余的最小数字是 3，它不能被更小的数整除，所以是素数。再将表中所有 3 的倍数全都划去。依次类推，如果表中剩余的最小数字是 m 时，m 就是素数。然后将表中所有 m 的倍数全部划去。像这样反复操作，就能依次枚举 n 以内的素数。

## 20 Valid Parentheses

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

- Open brackets must be closed by the same type of brackets.
- Open brackets must be closed in the correct order.
- Note that an empty string is also considered valid.

思路：
这个题用 Stack 是最简单的，利用了 Stack 的特性：**先进后出，后进先出**，以左半括号为，遇到左括号就把对应的右括号 push 进 stack 里面，遇到右括号就把栈里面的 pop 出来。

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

```
Input: [3,2,3]
Output: 3
```

**Example 2:**

```
Input: [2,2,1,1,1,2,2]
Output: 2
```

**思路一：**

看到这个题的时候，第一时间反应是利用 hashmap 存储，遍历 sums 中的元素，存在 hashmap 中，因为是返回众数，所以在存进 map 的时候可以直接做个判断，如果 key 的数量 > nums.length / 2，直接返回就行。

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

思路很巧妙，直接对 nums 进行排序，直接返回中间的元素即可。如果是数量最多的数字 (> n / 2 )，那么中间的数字必定是我们需要的结果。

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

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**Clarification:**

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().

## 21 Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

**思路一：** 链表合并

**1. 具体思路：**

设置一个 dummyhead，设置 pre 指针指向 head，然后两个指针 p,q 分别指向 l1,l2，当 p,q 不为空时，判断 p.val, q.val 的大小，如果 p.val 小，则 pre.next 指向 p，然后 p 指向下一节点。反之亦然。

当 p,q 中一条链表遍历完毕，则 pre.next 指向剩余的链表即可

最终返回 dummyhead.next

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

| 时间复杂度 | 空间复杂度 | 耗时 | 内存   |
| ---------- | ---------- | ---- | ------ |
| O(n)       | O(1)       | 6ms  | 41.3MB |

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

| 时间复杂度 | 空间复杂度 | 耗时 | 内存   |
| ---------- | ---------- | ---- | ------ |
| O(n)       | O(1)       | 6ms  | 41.3MB |

## 27 Remove Element

Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array** in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

**方法一：**

**思路：**

- 原地改**数组**
- 定义 k = 0；用来记录数组中不等于 val 的元素的个数
- 从头开始遍历数组（n =0），若数组中元素与 val 不相等（ 即 nums[n] != val）则将 nums[n] 赋给 nums[k], 然后将 k + 1; 然后执行下一次循环；若数组中元素与 val 值相等即（nums[n] != val），则执行下一次循环；直到循环执行结束
- 最后返回 k

**注意：**
这个题其实一开始我很晕，虽然很简单，但是要搞清楚题目的意思，不用管数组的长度和剩下的元素，只用替换掉前 k 个元素并且返回 k 就可以了！

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

```
Input: "Hello World"
Output: 5
```

思路：
这个题没难度，为什么我要写出来呢？就是需要注意，有一个小坑，如果字符串最后一个字符是空格的话，那么常规思路写的算法，看答案学到了一个 java 的方法：`.trim()`，目的是去除字符串两端的多余的空格。

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

```
Input: 1->1->2
Output: 1->2
```

**Example 2:**

```
Input: 1->1->2->3->3
Output: 1->2->3
```

思路：
很简单的一道题，考的就是对 Java 链表结点操作的熟练程度。在一开始我们定义一个指针 `cur`，利用 cur 判断当前与下一个结点的 val，相等的话就让 cur 的 next 指向 next.next。记住每次操作完将 cur 向后移动一个位置就可以了。

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

```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]\
```

思路：
这个题其实很简单，我们只需要创建一个新数组，然后把要合并的两个数组挨个遍历，然后挨个放进新数组就可以了。但是这种思路**太简单**，我们可以不生成新数组的情况下合并，从 nums1 的末尾从后往前放就可以了。
还有一种方法更加暴力，直接把两个数组拼接起来，然后 Array.sort() 排序，这里不再写方法。

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

```
Input:    1         1
         / \       / \
        2   3     2   3

       [1,2,3],   [1,2,3]
```

Output: true
**Example 2:**

```
Input:    1         1
         /           \
        2             2

      [1,2],     [1,null,2]
```

Output: false
**Example 3:**

```
Input:   1         1
        / \       / \
       2   1     1   2

      [1,2,1],   [1,1,2]
```

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

```
1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following `[1,2,2,null,3,null,3]` is not:

```
1
   / \
  2   2
   \   \
   3    3
```

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

```
You may assume the interval's end point is always bigger than its start point.
You may assume none of these intervals have the same start point.
```

**Example 1:**

```
Input: [ [1,2] ]

Output: [-1]

Explanation: There is only one interval in the collection, so it outputs -1.
```

**Example 2:**

```
Input: [ [3,4], [2,3], [1,2] ]

Output: [-1, 0, 1]

Explanation: There is no satisfied "right" interval for [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point;
For [1,2], the interval [2,3] has minimum-"right" start point.
```

**Example 3:**

```
Input: [ [1,4], [2,3], [3,4] ]

Output: [-1, 2, -1]

Explanation: There is no satisfied "right" interval for [1,4] and [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point.
```

## 442 Find All Duplicates in an Array

Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements that appear twice in this array.

**Could you do it without extra space and in O(n) runtime?**

**Example:**

```
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```

思路：题目中要求不要使用额外空间和 O(n) 时间复杂度，所以我们不能使用 HashMap 或者暴力搜索法。这道题其实思路也算是 HashMap，只不过我们使用数组自己本身作为键值对配对。遍历数组中的元素，然后将该数 - 1 作为 index（因为数组长度正好等于最大数），然后将 index 对应的原数取负数，继续遍历，若此 index 对应的数为负，则说明已经出现过。

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

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Example 2:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200831000511.png)

```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

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

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

思路：**先转成 int 再进行字符串反转** 或者利用下面的数学方法。

看起来这道题就这么解决了，但请注意，题目上还有这么一句

> 设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 `[−2^31,  2^31 − 1]`。

也就是说我们不能用 `long`存储最终结果，而且有些数字可能是合法范围内的数字，但是反转过来就超过范围了。
假设有 `1147483649`这个数字，它是小于最大的 32 位整数 `2147483647`的，但是将这个数字反转过来后就变成了 `9463847411`，这就比最大的 32 位整数还要大了，这样的数字是没法存到 int 里面的，所以肯定要返回 0（溢出了）。
甚至，我们还需要提前判断：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200831114818.png)

上图中，绿色的是最大 32 位整数
第二排数字中，橘子的是 `5`，它是大于上面同位置的 `4`，这就意味着 `5`后跟任何数字，都会比最大 32 为整数都大。
所以，我们到【最大数的 1/10】时，就要开始判断了
如果某个数字大于 `214748364`那后面就不用再判断了，肯定溢出了。
如果某个数字等于 `214748364`呢，这对应到上图中第三、第四、第五排的数字，需要要跟最大数的末尾数字比较，如果这个数字比 7 还大，说明溢出了。

对于负数也是一样的

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200831114859.png)

上图中绿色部分是最小的 32 位整数，同样是在【最小数的 1/10】时开始判断
如果某个数字小于 `-214748364`说明溢出了
如果某个数字等于 `-214748364`，还需要跟最小数的末尾比较，即看它是否小于 8

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

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

思路：[LeetCode 解法](https://leetcode-cn.com/problems/valid-anagram/solution/you-xiao-de-zi-mu-yi-wei-ci-by-leetcode/) 一种是排序，一种是哈希表。

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

```
Input: "abab"
Output: True
Explanation: It's the substring "ab" twice.
```

**Example 2:**

```
Input: "aba"
Output: False
```

**Example 3:**

```
Input: "abcabcabcabc"
Output: True
Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)
```

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

```
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

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

## 835 Image Overlap

Two images A and B are given, represented as binary, square matrices of the same size.  (A binary matrix has only 0s and 1s as values.)

We translate one image however we choose (sliding it left, right, up, or down any number of units), and place it on top of the other image.  After, the overlap of this translation is the number of positions that have a 1 in both images.

(Note also that a translation does **not** include any kind of rotation.)

What is the largest possible overlap?

**Example 1:**

```
Input:  A = [[1,1,0],
            [0,1,0],
            [0,1,0]]
        B = [[0,0,0],
            [0,1,1],
            [0,0,1]]
Output: 3
Explanation: We slide A to right by 1 unit and down by 1 unit.
```

思路：这个题是一个很傻逼的题，我们采取 Brute Force 的方法，四个 for 循环嵌套。
巧妙的点在于，我们 new 一个 N * 2 的数组用来存放偏移量，若果下次的偏移量一样的话，那么数组里的值++.

```java
class Solution {
    public int largestOverlap(int[][] A, int[][] B) {
        int N = A.length;
        int[][] count = new int[2 * N][2 * N];
        for(int i = 0; i < N; i++){
            for(int j = 0; j < N; j++){
                if(A[i][j] == 1){
                    for(int a = 0; a < N; a++){
                        for(int b =0; b < N; b++){
                            if(B[a][b] == 1){
                                count[i - a + N][j - b + N]++;
                            }
                        }
                    }
                }
            }
        }
        int res = 0;
        for(int[] row : count){
            for(int col : row){
                res = Math.max(res, col);
            }
        }
        return res;
    }
}
```

## Bulls and Cows

You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.

Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows.

Please note that both secret number and friend's guess may contain duplicate digits.

**Example 1:**

```
Input: secret = "1807", guess = "7810"

Output: "1A3B"

Explanation: 1 bull and 3 cows. The bull is 8, the cows are 0, 1 and 7.
```

**Example 2:**

```
Input: secret = "1123", guess = "0111"

Output: "1A1B"

Explanation: The 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow.
```

```java
class Solution {
    public String getHint(String secret, String guess) {
        if(secret.length() != guess.length()) return "4B";
        int N = secret.length();
        int bull = 0, cows = 0;
        int[] s = new int[10];
        int[] g = new int[10];
        for(int i = 0; i < N; i++){
            if(secret.charAt(i) == guess.charAt(i)){
                bull++;
            }else{
                s[secret.charAt(i) - '0']++;
                g[guess.charAt(i) - '0']++;
            }
        }
        for(int i = 0; i < s.length; i++){
            cows += Math.min(s[i], g[i]);
        }
        String res = bull + "A" + cows + "B";
        return res;
    }
}
```

## 1041. Robot Bounded In Circle

On an infinite plane, a robot initially stands at (0, 0) and faces north.  The robot can receive one of three instructions:

- "G": go straight 1 unit;
- "L": turn 90 degrees to the left;
- "R": turn 90 degress to the right.
  The robot performs the instructions given in order, and repeats them forever.

Return true if and only if there exists a circle in the plane such that the robot never leaves the circle.

**Example 1:**

```
Input: "GGLLGG"
Output: true
Explanation: 
The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).
When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.
```

**Example 2:**

```
Input: "GG"
Output: false
Explanation: 
The robot moves north indefinitely.
```

**Example 3:**

```
Input: "GL"
Output: true
Explanation: 
The robot moves from (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ...
```

看了很多人写的似乎都用死循环来判断最后是否会回到终点，其实有点多此一举了，因为只要走完一轮后，方向改变，即不是直走的话，最后无论再走多少轮总有一轮会走回终点。
下面看代码吧，最后困于环也就两种情况。

```java
public boolean isRobotBounded(String instructions) {
  
	int dir = 0; // 方向：0 上   1 右   2 下   3 左
	int x = 0;   // x 轴坐标
	int y = 0;   // y 轴坐标
	char ch;
	for(int i = 0; i < instructions.length(); i ++){
		ch = instructions.charAt(i); // 逐个读取字符
		if(ch == 'L'){
			if(dir == 0)
				dir = 3;
			else
				dir --;
		}
		if(ch == 'R'){
			if(dir == 3)
				dir = 0;
			else
				dir ++;
		}
		if(ch == 'G'){
			switch(dir){
			case 0: y ++; break;
			case 1: x ++; break;
			case 2: y --; break;
			case 3: x --; break;
			}
		}
	}
	// 情况 1: 走完一轮回到原点
	if(x == 0 && y == 0)
		return true;
	// 情况 2: 走完一轮，只要方向改变了（即不是直走了）, 最后不管走多少轮总会回到起点
	if(dir != 0)
		return true;

	return false;

}
```

## 229 Majority Element II

Given an integer array of size n, find all elements that appear more than `⌊ n/3 ⌋` times.

Note: The algorithm should run in linear time and in `O(1)` space.

**Example 1:**

```
Input: [3,2,3]
Output: [3]
```

**Example 2:**

```
Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
```

思路：这个题由于限制了时间和空间，所以我们不能采用常规的方法，这里介绍一种新方法：[**摩尔投票法**](https://leetcode-cn.com/problems/majority-element-ii/solution/liang-fu-dong-hua-yan-shi-mo-er-tou-piao-fa-zui-zh/)

明确一点就是众数最多只能有 2 个，如果两个数出现的次数分别为 a 和 b，`a>n/ 3`, `b>n/3`，这两个数为众数，设其他数字的出现次数为 c，那么 `a+b+c=n`，有 `a+b>2n/3`，`c<n/3`，因此最多只能有两个众数。

我们每次移除 3 个不同的数，因为 a 和 b 都大于 c，a-c 以及 b-c 都大于 0，最后剩下的就一定是众数。

但是众数有可能是 0 个或者 1 个，因此为了确认选出来的两个数是否是众数，还要进行一次遍历确认其出现次数是否大于 `n/3`

- 如果当前元素等于 cand1，则 cand1 次数加 1
- 如果当前元素等于 cand2，则 cand2 次数加 1
- 如果都不等，如果其中候选者的次数为 0，则更换候选
- 若 num 符合前面 3 个条件的其中一个，就要需要考虑下一个元素，因为如果满足了前面 3 个条件的其中一个 num 就成了候选者，如果前面 3 个条件都不满足，说明 cand1、cand2 与当前元素 num 各不相等，且 cand1 和 cand2 至少存在 1 个，那么同时移除一个 cand1、cand2 和 num
- 最后为了确保选出来的候选者是众数，还要一次遍历统计判断是否为众数

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> res = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        // 定义两个候选者和它们的票数
        int cand1 = 0,cand2 = 0;  
        int cnt1 = 0, cnt2 = 0;
        // 投票过程
        for (int num : nums) {
            // 如果是候选者 1，票数++
            if (num == cand1) {
                cnt1++;
                // 一遍遍历，如果你不想写 continue，你写多个 else if 也可以
                continue;
            }
            // 如果是候选者 2，票数++
            if (num == cand2) {
                cnt2++;
                continue;
            }
            // 既不是 cand1 也不是 cand2，如果 cnt1 为 0，那它就去做 cand1
            if (cnt1 == 0) {
                cand1 = num;
                cnt1++;
                continue;
            }
            // 如果 cand1 的数量不为 0 但是 cand2 的数量为 0，那他就去做 cand2
            if (cnt2 == 0) {
                cand2 = num;
                cnt2++;
                continue;
            }
            // 如果 cand1 和 cand2 的数量都不为 0，那就都-1
            cnt1--;
            cnt2--;
        }
        // 检查两个票数符不符合
        cnt1 = cnt2 = 0;
        for (int num : nums) {
            if (num == cand1) {
                cnt1++;
            } else if (num == cand2) {  
                // 这里一定要用 else if
                // 因为可能出现 [0,0,0] 这种用例，导致两个 cand 是一样的，写两个 if 结果就变为 [0,0] 了
                cnt2++;
            }
        }
        int n = nums.length;
        if (cnt1 > n / 3) {
            res.add(cand1);
        }
        if (cnt2 > n / 3) {
            res.add(cand2);
        }
        return res;
    }
}
```
