# Mark's LeetCode Record

- Palindrome Number
    - Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.  
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
- Two Sum
    - Given an array of integers, return indices of the two numbers such that they add up to a specific target.  
    You may assume that each input would have exactly one solution, and you may not use the same element twice.  
    Example:  
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