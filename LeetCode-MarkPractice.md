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