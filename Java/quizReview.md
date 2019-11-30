# AED Quiz Coding Review 
---
    Quiz 3 morning
     
---
- #### Quiz1
    - Give you a words, please output the inverted sequence of this words  
    Example:  
    Input “Hello”  
    output “olleH”.
    - 
        ```java
        return new StringBuffer(s).reverse().toString();
        return new StringBuilder(s).reverse().toString();
        ```

    - Give you an ArrayList with several Integers in it, return an ArrayList withall even number from the ArrayList given.  
    example:  
    Input: [1, 2, 3, 4, 5, 6]  
    Output: [2, 4, 6] 
     - 
        ```java
        public ArrayList<Integer> evenList(List<Integer> list){
            ArrayList<Integer> result = new ArrayList<>();
            for(int n : list){
                if(n % 2 == 0){
                    result.add(n);
                }
            }
            return result;
        }
        ```
- #### Quiz2
    - Given 2 lists of integers as input to the function, return the number of the elements existing in both arrays.  
    Example:  
    Input1 [2,3,5,7,27,1,6]  
    Input2 [8,55,77,5,9,6,1]  
    return 3  
    Explanation: The elements 1, 6 and 5 are present in both the inputlists, so return 3  
    Assumptions: No element in each input arrays would be repeated. If you want to test against your input, you could click"testagain custom input " and  input nums 1 in the first linenums2 in the second line into the textfield below.
    - 
        ```java
        int count = 0;
        for(int n : haha.nums1) {
			for(int m : haha.nums2) {
				if(n == m) {
					count++;
				}
			}
        }
        return count;
        ```
    - Give an int Array, return the first index that the sum of numbers in Array fromstart to this index is bigger than or equals sum of rest numbers.  
    Example[1,2,3,6,4,5]  
    return 3  
    because sum from index 0 to 3 is  1+2+3+6 >= 4+5  
    example:[1,6,5,4,9,7,2,4,3,3,6]  
    return 4  
    because 1+6+5+4+9 >= 7+2+4+3+3+6, and the index of 9 is 4
    - 
        ```java
        <!-- 写两个并行的循环 -->
        int[] nums = {1,2,2,6,2,9,7,6};
		for(int i =0;i<nums.length;++i) {
			int left = 0;
			int right = 0;
			for(int j=0;j<=i;++j)
				left+=nums[j];
			
			for(int j=i+1;j<nums.length;++j)
				right+=nums[j];
			
			if(left > right) {
				System.out.println(i);
				break;
			}	
		}
        ```

- #### Quiz3
    - Given an array, rotate the array to the right by k steps, where k is non-negative.   
    Example 1:  
    Input:[1,2,3,4,5,6,7] and k = 3  
    Output:[5,6,7,1,2,3,4]  
    Explanation:  
    rotate 1 steps to the right: [7,1,2,3,4,5,6]  
    rotate 2 steps to the right: [6,7,1,2,3,4,5]  
    rotate 3 steps to the right: [5,6,7,1,2,3,4]  
    Example 2:Input:[-1,-100,3,99] and k = 2  
    Output: [3,99,-1,-100]  
    Explanation:  
    rotate 1 steps to the right: [99,-1,-100,3]  
    rotate 2 steps to the right: [3,99,-1,-100]  
    - 
        ```java
        public void rotate(int[] nums, int k) {
            int n = k%nums.length;
            
            int[] temp = new  int[n];
            for(int i = 0; i < n; i++){
                temp[i] = nums[nums.length - n + i];
            }
            for(int i = nums.length -1; i >= n; i--){
                nums[i] = nums[i - n];           
            }
            for(int i = 0; i < n; i++){
                nums[i] =temp[i] ;
            }
        }
        ```

    - Students are asked to stand in non-decreasing order of heights in line.write a function when given an array tells how many students are not following this rule  
    Input: [1,1,4,2,1,3,1]  
    Output: 3  
    Explanation:   
    [..4, 2..] 2 is standing in front of 4 and violating the rule  
    [ ..2, 1..] 1 is standing in front of 2 and violating the rule   
    [ ..3, 1] 1 is standing in front of 3 and violating the rule  
    input:- the array of student heights  
    output:- (return type int)  
    number of students not following the rule  
    Note:  
    •) 1 <= heights.length <= 100  
    •) 1 <= heights[i] <= 10  
    •) The first person in the line is considered to always follow the rule asthere is no one in front  
    If you are going to test your case, the input(array)'s form is:1 2 3 4 5 6 7 8  
    (Splited by blank
    - 
        ```java
		int count = 0;
		for(int i = 1; i < nums.length; i++) {
			if(nums[i] < nums[i-1]) {
				count++;
			}
		}
		System.out.println(count);
        ```
- #### Quiz4
    - Car
    -  
        ```java
        class WagonR extends Car{
            private int carMileage;
            WagonR(int carMileage){
                super(false,"4");
                this.carMileage = carMileage;
            }

            public String getMileage(){
                return Integer.toString(CarMileage) + "kmpl";
            }
        }

        class HondaCity extends Car{
            private int carMileage;
            HondaCity(int carMileage){
                super(false, "4");
                this.carMileage = carMileage;
            }
            public String getMileage(){
                return Integer.toString(carMileage) + "kmpl";
            }
        }

        class InnovaCrysta extends Car{
            private int carMileage;
            InnovaCrysta(int carMileage){
                super(true, "5");
                this.carMileage = carMileage;
            }

            public String getMileage(){
                return Integer.toString(carMileage) + "kmpl";
            }
        }
        ```
- #### Quiz6
    - Given 2 lists of Integers as input to the function, return a SortedList(Descending order) containing integers included in both input lists.  Eg:input 1 : [2,3,5,7,27,1,6];  
    input 2 : [8,55,77,5,9,6,1];  
    answer: [6,5,1]  
    Explanation:  
    \- the elements 1,5,6 are present in both input lists  
    \- the returned values need to be in descending order thus,6(highest) to1(lowest).  
    Assumptions: no element in the input arrays would be repeated.  
    - 
        ```java
		Quiz1 q = new Quiz1();
		ArrayList<Integer> list = new ArrayList<>();
		for(int n : q.nums1) {
			for(int m :q.nums2) {
				if(n == m) {
					list.add(n);
				}
			}
		}
        // 这里是直接用Collections.sort();默认正序排序
        Collections.sort(list);
        // 这里是直接调用Comparator的内部倒序排序方法
        Collecitons.sort(list, Comparator.reverseOrdr());
        // 这里是重写的倒序排序方法
		Collections.sort(list, new Comparator<Integer>() {

			@Override
			public int compare(Integer o1, Integer o2) {
				
				return o1-o2;
			}
		});
		
		System.out.println(list);
        ```
    - 重写`Collections.sort()`中的`Comparator`方法，在Lab和作业中用到过。  
    - 什么时候需要重写`Comparator`方法：
        - 存进ArrayList里面的是一个Object，即Obejct里面还存有多条信息，例如：Object (id, name, item, price) 这个时候我们如果要比较Object中price的时候，我们就需要对`Comparator`方法进行重写。
            ```java
            Collections.sort(list, new Comparator<Object>() {

			@Override
			public int compare(Obejct o1, Object o2) {
				
				return o1.price-o2.price;
			    }
		    });
            ```
    - Bob is a kidnapper who wrote a ransom note, but now he is worried itwill be  traced back  to  him through his  handwring. He  found amagazine and wants to know if he can cut out whole words from itand use them to create an untraceable replica of his ransom note. Thewords in his note are case-sensive and he must use only whole wordsavailable in the magazine. He cannot use substrings or concatenaonto create the words he needs.  
    Given the words in the magazine and the words in the ransom note,print Yes  if  he  can  replicate  his  ransom note exactly  using wholewords from the magazine; otherwise, print No.  
    For example, the note is "Aack at dawn". The magazine contains only"aack at dawn". The magazine has all the right words, but there's acase mismatch. The answer is No.  
    Function Descripon  
    Complete  the checkMagazine  funcon in  the editor  below.  It  mustprint  Yes if the note can be formed using the magazine, or No.  
    checkMagazine has the following parameters:  
    - magazine: an array of strings, each a word in the magazine  
    - note: an array of strings, each a word in the ransom note  
    Input Format  
    The first  line contains  two  space-separated  integers, m and n, the numbers of words in the  magazine and the note.  
    The second line contains  m space-separated strings, each magazine[i].  
    The third line contains  n space-separated strings, each note[i].
    **Constraints**
    1<=m, n<=30000  
    1<=|magazine[i]|, |note[i]| <= 5.  
    Each word consists of English alphabec leers  (i.e. a to z and A to Z).  
    Output Format  
    Print Yes if he can use the magazine to create an untraceable replica ofhis ransom note. Otherwise, print No.  
    - 
        ```java
        if(magazine.length < note.length>){
            return false;
        }
        Map<String, Integer> magazines = new HashMap<>();
        for(String s : magazine){
            magazines.put(s, magazines.containsKey(s)? magazines.get(s)+1 : 1);
        }
        boolean match = true;
        for(String s : note){
            if(magazines.containsKey(s) && magazines.get(s) > 0){
                magazines.put(s,magazines.get(s)-1);
            }else{
                match = false;
                break;
            }
        }
        ```
- #### Quiz6
    - Write a Java program to find the first non-repeated character in a String which is not null. Your code must return the first non-repeated letter.If such letter does not exist, throw Runtime Exception"Didn't find any non-repeated Character" and print the exception message on the console  
    Sample Example:  
    swiss -> w  
    aabccd -> b
    - 
        ```java
        find_first_reapeted(char[] input){
            HashMap<Character,Integer> myhash = new HashMap<Character,Integer> ();
            for(int i=0;i<input.length;i++)
            {
            if(myhash.containsKey(input[i])
                myhash.put(input[i],2); //just put 2 even if it is more than 2
            else
                myhash.put(input[i],1);
            }
            for(int i=0;i<input.length;i++)
            {
            if(myhash.getValue(input[i])==1)
                return input[i];
            }
        }
        ```
    - Given an array containing n distinct numbers taken from 0, 1, 2,..., n, find the one that is missing from the array.  
    Example 1:  
    Input: [3,0,1]  
    Output: 2  
    Example 2:  
    Input: [9,6,4,2,3,5,7,0,1]  
    Output: 8