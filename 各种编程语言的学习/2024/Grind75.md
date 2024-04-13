### 

# Grind75

## Week 1 *reviewed /? not familiar  ? Do no understand

### *1. Leetcode1 Two Sum (Array) 

#### Solution 1

```python
class Solution:

​    def twoSum(self, nums: List[int], target: int) -> List[int]:

​        

​        for i in range(len(nums)):

​            for j in range(i+1,len(nums)):

​                if nums[i]+nums[j] == target:

​                    return [i,j]

​        return []
```

#### Solution2 ——Double Pointer

```py
# if it return the elements instead of the index then use this method
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        
        nums.sort()
        lo, hi = 0,len(nums)-1
        while lo < hi:
            if nums[lo]+nums[hi] == target:
                return [lo,hi]
            elif nums[lo]+nums[hi] < target:
                lo += 1
            elif nums[lo]+nums[hi] > target:
                hi -= 1
        
        return []
```

### nSUM 

```python
from typing import List

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        return self.nSumTarget(nums, 3, 0, 0)

    def nSumTarget(self, nums: List[int], n: int, start: int, target: int) -> List[List[int]]:
        sz = len(nums)
        res = []
        # 至少是 2Sum，且数组大小不应该小于 n
        if n < 2 or sz < n:
            return res
        # 2Sum 是 base case
        if n == 2:
            lo, hi = start, sz - 1
            while lo < hi:
                left, right = nums[lo], nums[hi]
                s = left + right
                if s < target:
                    while lo < hi and nums[lo] == left:
                        lo += 1
                elif s > target:
                    while lo < hi and nums[hi] == right:
                        hi -= 1
                else:
                    res.append([left, right])
                    while lo < hi and nums[lo] == left:
                        lo += 1
                    while lo < hi and nums[hi] == right:
                        hi -= 1
        else:
            # n > 2 时，递归计算 (n-1)Sum 的结果
            for i in range(start, sz):
                # 跳过重复元素
                if i > start and nums[i] == nums[i - 1]:
                    continue
                sub = self.nSumTarget(nums, n - 1, i + 1, target - nums[i])
                for arr in sub:
                    arr.append(nums[i])
                    res.append(arr)
        return res

```



### *2. Leetcode 21 [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) (Linked List) 

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

#### Solution1 - my own solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next = None)
        cur0 = dummy_head
        cur1 = list1
        cur2 = list2
        while cur1 and cur2:
            if cur1.val <= cur2.val:
                cur0.next = cur1
                cur1 = cur1.next
            else:
                cur0.next = cur2
                cur2 = cur2.next
            cur0 = cur0.next
        if cur2 == None:
            cur0.next = cur1
        elif cur1 == None:
            cur0.next = cur2

        return dummy_head.next
      
      
#have the same logic but a better one
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        cur = dummy = ListNode()
        while list1 and list2:               
            if list1.val < list2.val:
                cur.next = list1
                list1, cur = list1.next, list1
            else:
                cur.next = list2
                list2, cur = list2.next, list2
                
        if list1 or list2:
            cur.next = list1 if list1 else list2
            
        return dummy.next
```



> [!NOTE]
>
> The primary purpose of the `dummy` node is to provide a simple and effective way to return the head of the newly merged list without needing special case handling for the head node. Here's why the `dummy` node is beneficial:



### *3. LeetCode 121 [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) (Array) 

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith`day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

#### Solution 1 - out of time limited

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        sell_price,buy_price,max_profit = 0,prices[0],0
        for i in range(len(prices)):
            buy_price = sell_price = prices[i]
            for j in range(i+1,len(prices)):
                if prices[j] > sell_price:
                    sell_price = prices[j]

            if sell_price - buy_price > max_profit:
                max_profit = sell_price - buy_price
            

        return max_profit
        
```

#### Solution 2 - O(n)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = prices[0]
        max_profit = 0

        for price in prices:

            if price < min_price:
                min_price = price

            current_profit = price - min_price

            if current_profit > max_profit:
                max_profit = current_profit
            
        return max_profit
```

### *4. LeetCode [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) (Stack) 

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

```python
class Solution:
    def isValid(self, s: str) -> bool:
        left = []

        for c in s:
            if c == '{' or c == '[' or c =='(':
                left.append(c) 
            else:
                if left and leftOf(c) == left[-1]:
                    left.pop()
                else:
                    return False

        return not left


def leftOf(c: str):
    if c == '}':
        return '{'
    elif c ==']':
        return '['
    elif c == ')':
        return '('
        
```

### *5. LeetCode [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) （Array-Double pointer）

A phrase is a **palindrome**(回文) if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` *if it is a **palindrome**, or* `false` *otherwise*.

#### Double Pointer

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s2 = []
        for c in s:
            if c.isalnum():
                s2.append(c.lower())
                
        lo,hi = 0,len(s2)-1
        while lo < hi :
            if s2[lo] != s2[hi]:
                return False
            lo += 1
            hi -=1
        return True

```

> [!NOTE]
>
> **一些实用的函数：**
>
> ```
> char c
> c.lower() #get the lower letter of char c
> ```
>
> **str需要通过迭代的方式加入字符数组**



### ？6. LeetCode [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) (递归) （Tree）？

Given the `root` of a binary tree, invert the tree, and return *its root*.

~~~py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return root

        self.invertTree(root.left)
        self.invertTree(root.right)

        root.left,root.right = root.right,root.left ## 这里如果拆分成两行赋值的话，会有一定的改变
```
>>> a = 1
>>> b = 3
>>> a = b
>>> b = a
>>> a,b
(3, 3)
>>> a = 1
>>> b = 3
>>> a,b = b,a
>>> a,b
(3, 1)
```
        return root
        
        
~~~

### *7. LeetCode [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/) (Hash) 

Given two strings `s` and `t`, return `true` *if* `t` *is an anagram of* `s`*, and* `false` *otherwise*.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

#### Solution1 as an array O(nlogn)

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        
        sorted_s = sorted(s)
        sorted_t = sorted(t)
        return sorted_s == sorted_t
            
        return True
        
```

#### Solution2 Hash Table  O(n) 

-When you try to calculate the number of characters in a string

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        count = defaultdict(int)
        
        # Count the frequency of characters in string s
        for x in s:
            count[x] += 1
        
        # Decrement the frequency of characters in string t
        for x in t:
            count[x] -= 1
        
        # Check if any character has non-zero frequency
        for val in count.values():
            if val != 0:
                return False
        
        return True
      
      
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        count = [0] * 26 ##仅仅是创建hash table的方式不同
        
        # Count the frequency of characters in string s
        for x in s:
            count[ord(x) - ord('a')] += 1
        
        # Decrement the frequency of characters in string t
        for x in t:
            count[ord(x) - ord('a')] -= 1
        
        # Check if any character has non-zero frequency
        for val in count:
            if val != 0:
                return False
        
        return True
```

### *8. LeetCode [704. Binary Search](https://leetcode.com/problems/binary-search/) (Array)    O(logn)   

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with **`O(log n)`** runtime complexity.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left,right = 0, len(nums)-1
        

        while left <= right:
            mid = (left + right)//2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1 #这里的+1和下面的-1很重要
            else:
                right = mid - 1

        return -1
```

### /？9.  LeetCode [733. Flood Fill](https://leetcode.com/problems/flood-fill/) （Graph）

```python
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        
        start_color = image[sr][sc]
        
        def flood_fill(x, y):
            if x < 0 or x >= len(image): return
            if y < 0 or y >= len(image[0]): return
            
            if image[x][y] == color: return
            if image[x][y] != start_color: return
            
            image[x][y] = color
            
            flood_fill(x-1, y)
            flood_fill(x+1, y)
            flood_fill(x, y+1)
            flood_fill(x, y-1)
        
        flood_fill(sr, sc)
        return image
```

> [!NOTE]
>
> 在Python中，函数内部定义的子函数（也称为嵌套函数或闭包）可以直接访问其父函数作用域中的变量。这是因为Python支持词法作用域（lexical scoping），即函数可以访问其被定义时的作用域中的变量，而不仅仅是在其被调用时的作用域中的变量。

### * 10. LeetCode [409. Longest Palindrome](https://leetcode.com/problems/longest-palindrome/)

Given a string `s` which consists of lowercase or uppercase letters, return *the length of the **longest palindrome*** that can be built with those letters.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome here.

```py
# Hash Table
class Solution:
    def longestPalindrome(self, s: str) -> int:
        length = 0
        count = defaultdict(int)
        flag = 0
        # Count the frequency of characters in string s
        for x in s:
            count[x] += 1

        for val in count.values():
            if val % 2 == 0:
                length += val

            else:
                flag = 1
                if val // 2 > 0:
                    length += (val // 2 * 2)
                    
        return length + flag
      
      
## A more simple solution
def longestPalindrome_set(s):
    ss = set()
    for letter in s:
        if letter not in ss:
            ss.add(letter)
        else:
            ss.remove(letter)
    if len(ss) != 0:
        return len(s) - len(ss) + 1
    else:
        return len(s)
# OR

def longestPalindrome(self, s: str) -> int:
    unpairedLetters = set({})
    palindromeLength = 0

    for letter in s:
        if letter in unpairedLetters: 
            # found a pair
            palindromeLength += 2
            unpairedLetters.remove(letter)
        else:
            unpairedLetters.add(letter)

    if len(unpairedLetters) > 0: 
        # can use at most one unpaired letter, in the middle of the palindrome
        palindromeLength += 1
    
    return palindromeLength
```

### * 11. LeetCode [169. Majority Element](https://leetcode.com/problems/majority-element/)

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

#### Solution1 Sorted O(n log n) 

```
#Solution1

##Cuz You may assume that the majority element always exists in the array, so this solution is kinda efficient!!

class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums.sort()
        n = len(nums)
        return nums[n//2]
```

#### Solution2 O(n) Boyer-Moore Majority Vote Algorithm

```py
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = 0
        
        for num in nums:
            if count == 0:
                candidate = num
            
            if num == candidate:
                count += 1
            else:
                count -= 1
        
        return candidate
```

#### Solution3 Hash Table O(n)

### Two ways to create a hash table via dictionary

#### defaultdict()

```py
from collections import defaultdict

# 使用 list 作为默认工厂函数
d = defaultdict(list) #常用的有int list set

class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        n = len(nums)
        m = defaultdict(int)
        
        for num in nums:
            m[num] += 1
        
        n = n // 2
        for key, value in m.items():
            if value > n:
                return key
        
        return 0
```

#### Create a general dictionary 

```py
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = {}
        majority_element = nums[0]

        for num in nums:
            if num not in count:
                count[num] = 0
            count[num] += 1
            
            if count[num] > len(nums) // 2:
                majority_element = num
                break

        return majority_element

```

### *12. Leet Code[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cur = head
        pre = None
        temp = head
        while cur:
          
            temp = cur.next #save the next node when the cur.next changes
            cur.next = pre # reverse
            pre = cur # the head of the reversed one    
            cur = temp # go to the next node
           
        return pre
```



### *13.LeetCode [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

```py
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        count = defaultdict(int)
        for num in nums:
            count[num] += 1
            if count[num] > 1:
                return True
                
        return False
```



### 14. LeetCode [15. 3Sum](https://leetcode.com/problems/3sum/)

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

> ### Two Pointer Approach:
>
> The basic thinking logic for this is: Fix any one number in sorted array and find the other two numbers after it. The other two numbers can be easily found using two pointers (as array is sorted) and two numbers should have sum = -1*(fixed number).
>
> - Traverse the array and fix a number at every iteration.
> - If number fixed is +ve, break there because we can't make it zero by searching after it.
> - If number is getting repeated, ignore the lower loop and continue. This is for unique triplets. We want the last instance of the fixed number, if it is repeated.
> - Make two pointers high and low, and initialize sum as 0.
> - Search between two pointers, just similiar to binary search. Sum = num[i] + num[low] + num[high].
> - If sum is -ve, means, we need more +ve numbers to make it 0, increament low (low++).
> - If sum is +ve, means, we need more -ve numbers to make it 0, decreament high (high--).
> - If sum is 0, that means we have found the required triplet, push it in answer vector.
> - Now again, to avoid duplicate triplets, we have to navigate to last occurences of num[low] and num[high] respectively. Update the low and high with last occurences of low and high.

```py
def threeSum(self, nums):
    nums.sort()
    result = []
    for left in range(len(nums) - 2): # renamed this to left because this will always be the leftmost pointer in the triplet
        if left > 0 and nums[left] == nums[left - 1]: # this step makes sure that we do not have any duplicates in our result output
            continue 
        mid = left + 1 # renamed this to mid because this is the pointer that is between the left and right pointers
        right = len(nums) - 1
        while mid < right:
            curr_sum = nums[left] + nums[mid] + nums[right]
            if curr_sum < 0:
                mid += 1 
            elif curr_sum > 0:
                right -= 1
            else:
                result.append([nums[left], nums[mid], nums[right]])
                while mid < right and nums[mid] == nums[mid + 1]: # Another conditional for not calculating duplicates
                    mid += 1
                while mid < right and nums[right] == nums[right - 1]: # Avoiding duplicates check
                    right -= 1
                mid += 1
                right -= 1
    return result
```



```py
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        

	    res = set()
    
	    #1. Split nums into three lists: negative numbers, positive numbers, and zeros
	    n, p, z = [], [], []
	    for num in nums:
	    	if num > 0:
	    		p.append(num)
	    	elif num < 0: 
	    		n.append(num)
	    	else:
	    		z.append(num)
    
	    #2. Create a separate set for negatives and positives for O(1) look-up times
	    N, P = set(n), set(p)
    
	    #3. If there is at least 1 zero in the list, add all cases where -num exists in N and num exists in P
	    #   i.e. (-3, 0, 3) = 0
	    if z:
	    	for num in P:
	    		if -1*num in N:
	    			res.add((-1*num, 0, num))
    
	    #3. If there are at least 3 zeros in the list then also include (0, 0, 0) = 0
	    if len(z) >= 3:
	    	res.add((0,0,0))
    
	    #4. For all pairs of negative numbers (-3, -1), check to see if their complement (4)
	    #   exists in the positive number set
	    for i in range(len(n)):
	    	for j in range(i+1,len(n)):
	    		target = -1*(n[i]+n[j])
	    		if target in P:
	    			res.add(tuple(sorted([n[i],n[j],target])))
    
	    #5. For all pairs of positive numbers (1, 1), check to see if their complement (-2)
	    #   exists in the negative number set
	    for i in range(len(p)):
	    	for j in range(i+1,len(p)):
	    		target = -1*(p[i]+p[j])
	    		if target in N:
	    			res.add(tuple(sorted([p[i],p[j],target])))
    
	    return res
```

### * 15. LeetCode [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) ***

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

```py
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        length=len(nums)
        sol=[1]*length
        pre = 1
        post = 1
        for i in range(length):
            sol[i] *= pre
            pre = pre*nums[i]
            sol[length-i-1] *= post
            post = post*nums[length-i-1]
        return(sol)
```

### *16. LeetCode [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

Given the `head` of a singly linked list, return *the middle node of the linked list*.

If there are two middle nodes, return **the second middle** node.

```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cur = slow = head
        i = 1
        while cur:
            if i % 2 == 0:
                slow = slow.next
            cur = cur.next
            i += 1
        return slow

```

```py
# Time Complexity :  O(n)
# Space Complexity : O(1)
class Solution(object):
    def middleNode(self, head):
        # We need two pointers, one is head with one step each iteration, and the other is tmp with two steps each iteration.
        temp = head
        while temp and temp.next:
            # In each iteration, we move head one node forward and we move temp two nodes forward...
            head = head.next
            temp = temp.next.next
        # When temp reaches the last node, head will be exactly at the middle point...
        return head
```

### 17. LeetCode [146. LRU Cache](https://leetcode.com/problems/lru-cache/) （Using the dic and its order feature）***

[带你手撸 LRU 算法](https://labuladong.github.io/algo/di-yi-zhan-da78c/shou-ba-sh-daeca/suan-fa-ji-8674e/#二、lru-算法设计)

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

```python
class LRUCache:

    def __init__(self, capacity: int):
        self.dict = {}
        self.capacity = capacity   

    def get(self, key: int) -> int:
        if key not in self.dict:
            return -1
        val = self.dict.pop(key)  #Remove it first before inserting it at the end again
        self.dict[key] = val   
        return val        

    def put(self, key: int, value: int) -> None:
        if key in self.dict:    
            self.dict.pop(key)
        else:
            if len(self.dict) == self.capacity:
                del self.dict[next(iter(self.dict))]         
        self.dict[key] = value
```

### 18. LeetCode [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key =lambda x: x[0]) ##这里排序是必须的 In python, use sort method to a list costs O(nlogn), where n is the length of the list.
        merged =[]
        for interval in intervals:
			# if the list of merged intervals is empty 
			# or if the current interval does not overlap with the previous,
			# simply append it.
            if not merged or merged[-1][-1] < interval[0]:
                merged.append(interval)
			# otherwise, there is overlap,
			#so we merge the current and previous intervals.
            else:
                merged[-1][-1] = max(merged[-1][-1], interval[-1])
        return merged
```

### 19. LeetCode [75. Sort Colors](https://leetcode.com/problems/sort-colors/) -Pointer ***

Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

```python
class Solution:
    def sortColors(self, nums):
        red, white, blue = 0, 0, len(nums)-1
    
        while white <= blue:
            if nums[white] == 0:
                nums[red], nums[white] = nums[white], nums[red]
                white += 1
                red += 1
            elif nums[white] == 1:
                white += 1
            else:
                nums[white], nums[blue] = nums[blue], nums[white]
                blue -= 1
            
```

 ### 20. LeetCode [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return *the maximum amount of water a container can store*.

**Notice** that you may not slant the container.

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        max_water = 0
        left,right = 0, len(height)-1
        while left < right:
            h = min(height[left],height[right])
            x = right-left 
            max_water = max(x*h,max_water)
            if height[left] <= height[right]:
                left += 1
            else:
                right -= 1

        return max_water
        
```

### 21. LeetCode [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

```python
class MyQueue:

    def __init__(self):
        self.in_stk = []
        self.out_stk = []
        

    def push(self, x: int) -> None:
        self.in_stk.append(x)

    def pop(self) -> int:
        self.peek()
        return self.out_stk.pop()
       

    def peek(self) -> int: #在这里出栈入栈实现queue功能
        if not self.out_stk: # out_stk 中没有值时
            while self.in_stk:
                self.out_stk.append(self.in_stk.pop())
        
        return self.out_stk[-1]

        

    def empty(self) -> bool:
        return not len(self.in_stk) and not len(self.out_stk) 
        
        


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```



### 22. LeetCode [67. Add Binary](https://leetcode.com/problems/add-binary/)

> [!NOTE]
>
> ```python
> "".join(s[::-1]) # join string array continuly
> 	#s[::-1] means reverse the array 's'
> ```
>
> ```python
> val += int(a[i]) if i < len(a) else 0 # the if syntax
> ```
>
> ```python
> a_int = int(a,2) #transfer the string from 2-
> ```
>
> 

Given two binary strings `a` and `b`, return *their sum as a binary string*.

**Example 1:**

```
Input: a = "11", b = "1"
Output: "100"
```

```python
#my solution-- use the functions of python itself
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        a_int,b_int = int(a,2),int(b,2)
        c_int = a_int + b_int
        return str(bin(c_int)[2:])


# General Solution
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        a = a[::-1]
        b = b[::-1]

        s = []

        carry = 0
        i = 0

        while i < max(len(a),len(b)) or carry >0:
            val = carry
            val += int(a[i]) if i < len(a) else 0
            val += int(b[i]) if i < len(b) else 0
            s.append(str(val%2))
            carry = val//2
            i += 1

        return "".join(s[::-1])


```



### 23. LeetCode [383. Ransom Note](https://leetcode.com/problems/ransom-note/) Hash Table

Given two strings `ransomNote` and `magazine`, return `true` *if* `ransomNote` *can be constructed by using the letters from* `magazine` *and* `false` *otherwise*.

Each letter in `magazine` can only be used once in `ransomNote`.

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        d = defaultdict(int)

        for c in magazine:
            d[c]+=1

        for c in ransomNote:
            if c in d and d[c] !=0:
                d[c] -=1
            else:
                return False


        return True
                        
```



### 24. LeetCode [278. First Bad Version](https://leetcode.com/problems/first-bad-version/) Binary Search

> [!NOTE]
>
> - Time complexity: **O(log N)**
> - Space complexity: **O(1)**

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

```python
# The isBadVersion API is already defined for you.
# def isBadVersion(version: int) -> bool:

class Solution:
    def firstBadVersion(self, n: int) -> int:
        left,right = 1,n
        
        while left <= right:
            mid = (left + right )//2
            if isBadVersion(mid) == False:
                left = mid + 1  #同样，这里的+1和-1很重要，能够最大化的省事
            else:
                right = mid - 1

        return left
        
```



### 25. LeetCode[39. Combination Sum](https://leetcode.com/problems/combination-sum/)

[回溯算法解决排列、组合、子集问题](https://labuladong.online/algo/essential-technique/permutation-combination-subset-all-in-one/)

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

### [48. Rotate Image](https://leetcode.com/problems/rotate-image/) （顺时针or逆时针旋转数组）

- 先镜面对称（顺时针就沿左上角镜面堆成逆时针就右上角镜面对称），然后反转各行

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

```python
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。

def rotate(matrix: List[List[int]]):
    n = len(matrix)
    # 先沿对角线镜像对称二维矩阵
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # 然后反转二维矩阵的每一行
    for row in matrix:
        reverse(row)

# 反转一维数组
def reverse(arr: List[int]):
    i, j = 0, len(arr)-1
    while j > i:
        arr[i], arr[j] = arr[j], arr[i]
        i+=1
        j-=1

```

```python
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。

# 将二维矩阵原地逆时针旋转 90 度
def rotate2(matrix: List[List[int]]) -> None:
    n = len(matrix)
    # 沿左下到右上的对角线镜像对称二维矩阵
    for i in range(n):
        for j in range(n - i):
            # swap(matrix[i][j], matrix[n-j-1][n-i-1])
            temp = matrix[i][j]
            matrix[i][j] = matrix[n - j - 1][n - i - 1]
            matrix[n - j - 1][n - i - 1] = temp
    # 然后反转二维矩阵的每一行
    for row in matrix:
        reverse(row)

def reverse(arr: List[int]) -> None:
    # 见上文
    pass

```



### [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) （螺旋遍历二维数组）

Given an `m x n` `matrix`, return *all elements of the* `matrix` *in spiral order*.

```python
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。

from typing import List

def spiralOrder(matrix: List[List[int]]) -> List[int]:
    m = len(matrix)
    n = len(matrix[0])
    upper_bound = 0
    lower_bound = m - 1
    left_bound = 0
    right_bound = n - 1
    res = []
    # res.length == m * n 则遍历完整个数组
    while len(res) < m * n:
        if upper_bound <= lower_bound:
            # 在顶部从左向右遍历
            for j in range(left_bound, right_bound + 1):
                res.append(matrix[upper_bound][j])
            # 上边界下移
            upper_bound += 1
        
        if left_bound <= right_bound:
            # 在右侧从上向下遍历
            for i in range(upper_bound, lower_bound + 1):
                res.append(matrix[i][right_bound])
            # 右边界左移
            right_bound -= 1
        
        if upper_bound <= lower_bound:
            # 在底部从右向左遍历
            for j in range(right_bound, left_bound - 1, -1):
                res.append(matrix[lower_bound][j])
            # 下边界上移
            lower_bound -= 1
        
        if left_bound <= right_bound:
            # 在左侧从下向上遍历
            for i in range(lower_bound, upper_bound - 1, -1):
                res.append(matrix[i][left_bound])
            # 左边界右移
            left_bound += 1
    
    return res

```

### 滑动窗口（sliding window）框架

```python
# 注意：python 代码由 chatGPT🤖 根据我的 cpp 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 cpp 代码对比查看。

# 滑动窗口算法框架
def slidingWindow(s: str):
    # 用合适的数据结构记录窗口中的数据，根据具体场景变通
    # 比如说，我想记录窗口中元素出现的次数，就用 map
    # 我想记录窗口中的元素和，就用 int
    window = dict()
    
    left = 0
    right = 0
    while right < len(s):
        # c 是将移入窗口的字符
        c = s[right]
        window[c] = window.get(c, 0) + 1
        # 增大窗口
        right += 1
        # 进行窗口内数据的一系列更新
        #...

        #/*** debug 输出的位置 ***/
        # 注意在最终的解法代码中不要 print
        # 因为 IO 操作很耗时，可能导致超时
        # print(f"window: [{left}, {right})")
        #/********************/

        # 判断左侧窗口是否要收缩
        while left < right and "window needs shrink":
            # d 是将移出窗口的字符
            d = s[left]
            window[d] -= 1
            if window[d] == 0:
                del window[d]
            # 缩小窗口
            left += 1
            # 进行窗口内数据的一系列更新
            #...

```

> [!NOTE]
>
> 1、我们在字符串 `S` 中使用双指针中的左右指针技巧，初始化 `left = right = 0`，把索引**左闭右开**区间 `[left, right)` 称为一个「窗口」。
>
> 为什么要「左闭右开」区间
>
> **理论上你可以设计两端都开或者两端都闭的区间，但设计为左闭右开区间是最方便处理的**。
>
> 因为这样初始化 `left = right = 0` 时区间 `[0, 0)` 中没有元素，但只要让 `right` 向右移动（扩大）一位，区间 `[0, 1)` 就包含一个元素 `0` 了。
>
> 如果你设置为两端都开的区间，那么让 `right` 向右移动一位后开区间 `(0, 1)` 仍然没有元素；如果你设置为两端都闭的区间，那么初始区间 `[0, 0]` 就包含了一个元素。这两种情况都会给边界处理带来不必要的麻烦。

2、我们先不断地增加 `right` 指针扩大窗口 `[left, right)`，直到窗口中的字符串符合要求（包含了 `T` 中的所有字符）。

3、此时，我们停止增加 `right`，转而不断增加 `left` 指针缩小窗口 `[left, right)`，直到窗口中的字符串不再符合要求（不包含 `T` 中的所有字符了）。同时，每次增加 `left`，我们都要更新一轮结果。

4、重复第 2 和第 3 步，直到 `right` 到达字符串 `S` 的尽头。

这个思路其实也不难，**第 2 步相当于在寻找一个「可行解」，然后第 3 步在优化这个「可行解」，最终找到最优解**，也就是最短的覆盖子串。左右指针轮流前进，窗口大小增增减减，就好像一条毛毛虫，一伸一缩，不断向右滑动，这就是「滑动窗口」这个名字的来历。



### [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window*** 

***substring\***

 *of* `s` *such that every character in* `t` *(**including duplicates**) is included in the window*. If there is no such substring, return *the empty string* `""`.



The testcases will be generated such that the answer is **unique**.

```python
# 注意：python 代码由 chatGPT🤖 根据我的 cpp 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        from collections import Counter
        need = Counter(t)
        window = Counter()

        left, right = 0, 0
        valid = 0
        # 记录最小覆盖子串的起始索引及长度
        start, length = 0, float('inf') 

        while right < len(s):
            # c 是将移入窗口的字符
            c = s[right]
            right += 1
            # 进行窗口内数据的一系列更新
            if c in need:
                window[c] += 1
                if window[c] == need[c]:
                    valid += 1

            # 判断左侧窗口是否要收缩
            while valid == len(need): 

                # 在这里更新最小覆盖子串
                if right - left < length:
                    start = left
                    length = right - left
                # d 是将移出窗口的字符
                d = s[left]
                left += 1
                # 进行窗口内数据的一系列更新
                if d in need:
                    if window[d] == need[d]:
                        valid -= 1
                    window[d] -= 1 

        # 返回最小覆盖子串
        return '' if length == float('inf') else s[start:start+length]
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=minimum-window-substring

```

> [!TIP]
>
> 一种排序方式，能够保存原来的index
>
> ```python
> sorted_nums2 = sorted((num, i) for i, num in enumerate(nums2))
> ```
>
> ```py
> class Solution:
>     def advantageCount(self, nums1: List[int], nums2: List[int]) -> List[int]:
>         n = len(nums1)
>         sorted_nums1 = sorted(nums1)
>         sorted_nums2 = sorted((num, i) for i, num in enumerate(nums2))
>         left, right = 0, n - 1
>         res = [0] * n
> 
>         # 修改这里：遍历整个 sorted_nums2
>         for val, i in sorted(sorted_nums2, reverse=True):
>             # 不再需要单独声明 maxval 和 i，直接在 for 循环中解包
>             if val < sorted_nums1[right]:
>                 res[i] = sorted_nums1[right]
>                 right -= 1
>             else:
>                 res[i] = sorted_nums1[left]
>                 left += 1
> 
>         return res
> 
> ```



> [!TIP]
>
> 选择随机数的语法
>
> ```python
> import random
> target = rand.randint(1, preSum[n - 1])
> ```

