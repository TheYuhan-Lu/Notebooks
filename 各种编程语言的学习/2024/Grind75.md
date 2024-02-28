

# Grind75

## Week 1

### Leetcode1 Two Sum (Array)

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

### Leetcode 21 [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) (Linked List)

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

### LeetCode 121 [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) (Array)

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

### LeetCode [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) (Stack)

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

### LeetCode [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) （Array-Double pointer）

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

### LeetCode [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) (递归) （Tree）

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

### LeetCode [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/) (Hash)

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

### LeetCode [704. Binary Search](https://leetcode.com/problems/binary-search/) (Array)    O(logn)

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

### LeetCode [733. Flood Fill](https://leetcode.com/problems/flood-fill/) （Graph）

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



### LeetCode [409. Longest Palindrome](https://leetcode.com/problems/longest-palindrome/)

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

### LeetCode [169. Majority Element](https://leetcode.com/problems/majority-element/)

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

#### Solution1 Sorted O(n log n) 

```
#Solution1
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums.sort()
        n = len(nums)
        return nums[n//2]
```

#### Solution2 O(n)

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

### Leet Code[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

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



### LeetCode [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

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



### LeetCode [15. 3Sum](https://leetcode.com/problems/3sum/)

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

### LeetCode [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) ***

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

### LeetCode [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

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

### LeetCode [146. LRU Cache](https://leetcode.com/problems/lru-cache/) （Using the dic and its order feature）***

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

### LeetCode [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

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

### LeetCode [75. Sort Colors](https://leetcode.com/problems/sort-colors/) -Pointer ***

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

 ### LeetCode [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

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

### LeetCode [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

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



### LeetCode [67. Add Binary](https://leetcode.com/problems/add-binary/)

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



### LeetCode [383. Ransom Note](https://leetcode.com/problems/ransom-note/) Hash Table

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



### LeetCode [278. First Bad Version](https://leetcode.com/problems/first-bad-version/) Binary Search

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



### LeetCode[39. Combination Sum](https://leetcode.com/problems/combination-sum/)

[回溯算法解决排列、组合、子集问题](https://labuladong.online/algo/essential-technique/permutation-combination-subset-all-in-one/)

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

