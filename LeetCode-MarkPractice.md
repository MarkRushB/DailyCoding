# Mark's LeetCode Record <!-- omit in toc -->
- [Attention](#attention)
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

## Attention
- [刷题需要注意的小细节](LeetCode-Attention.md)


## 9 [Palindrome Number](https://leetcode.com/problems/palindrome-number/)

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
## 1 [Two Sum](https://leetcode.com/problems/two-sum/)
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
## 14 [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)
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

## 21 [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
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

## 27 [Remove Element](https://leetcode.com/problems/remove-element/)
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
## 58 [Length of Last Word](https://leetcode.com/problems/length-of-last-word/)

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
## 83 [Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)
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

## 88 [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

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