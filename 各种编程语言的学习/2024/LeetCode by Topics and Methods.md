# LeetCode by Topics and Methods

## Array

### <u>**- Double Pointer**</u>

### <u>-- Fast and Slow Pointer</u>

- 原地修改数组

🧡❤️💚

### [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) 💚

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**. Then return *the number of unique elements in* `nums`.

Consider the number of unique elements of `nums` to be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the unique elements in the order they were present in `nums` initially. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
    # 维护 nums[0..slow] 无重复
        slow = 0 
        fast = 1
        while fast < len(nums):
            if nums[fast] != nums[slow]:
                slow += 1
            # 维护 nums[0..slow] 无重复
                nums[slow] = nums[fast]
            fast += 1
    # 数组长度为索引 + 1
        return slow + 1

```



### [27. Remove Element](https://leetcode.com/problems/remove-element/) 💚

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

```python

class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        fast, slow = 0, 0
        while fast < len(nums):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow
```



> [!NOTE]
>
> 注意这里和有序数组去重的解法有一个细节差异，我们这里是先给 `nums[slow]` 赋值然后再给 `slow++`，这样可以保证 `nums[0..slow-1]` 是不包含值为 `val` 的元素的，最后的结果数组长度就是 `slow`。



### [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/) 💚

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # 去除 nums 中的所有 0
        # 返回去除 0 之后的数组长度
        p = self.removeElement(nums, 0)
        # 将 p 之后的所有元素赋值为 0
        for i in range(p, len(nums)):
            nums[i] = 0
            
    # 双指针技巧，复用 [27. 移除元素] 的解法。
    def removeElement(self, nums: List[int], val: int) -> int:
        fast = 0
        slow = 0
        while fast < len(nums):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow


```

### <u>-- Left and Right Pointer</u>

- 二分查找
- 两数之和
- 反转数组
- 回文串判断

###  [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)🧡

Given a **1-indexed** array of integers `numbers` that is already ***sorted in non-decreasing order\***, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return *the indices of the two numbers,* `index1` *and* `index2`*, **added by one** as an integer array* `[index1, index2]` *of length 2.*

The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.

Your solution must use only constant extra space.

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
    	left, right = 0, len(nums) - 1
    	while left < right:
    	    sum = nums[left] + nums[right]
    	    if sum == target:
    	        # 题目要求的索引是从 1 开始的
    	        return [left + 1, right + 1]
    	    elif sum < target:
    	        left += 1 # 让 sum 大一点
    	    else:
    	        right -= 1 # 让 sum 小一点
    	return [-1, -1]
```



### [344. Reverse String](https://leetcode.com/problems/reverse-string/) 💚

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        left, right = 0, len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
```

### [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) 🧡

Given a string `s`, return *the longest* 

*palindromic* *substring*  in `s`.

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

```python
class Solution:

    def longestPalindrome(self, s: str) -> str:
        def palindrome(s, l, r):
            while (l >= 0 and r < len(s) and s[l] == s[r]):
                l -= 1
                r += 1
            return s[l+1:r]

        res = ""
        for i in range(len(s)):
        # 以 s[i] 为中心的最长回文子串
            s1 = palindrome(s, i, i)
        # 以 s[i] 和 s[i+1] 为中心的最长回文子串
            s2 = palindrome(s, i, i + 1)
        # res = longest(res, s1, s2)
            res = res if len(res) > len(s1) else s1
            res = res if len(res) > len(s2) else s2
        return res



```



### [83. Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/) 🧡

Given the `head` of a sorted linked list, *delete all duplicates such that each element appears only once*. Return *the linked list **sorted** as well*.

 

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if head == None:
            return None
        slow = head
        fast = head
        while fast != None:
            if fast.val != slow.val:
                # nums[slow] = nums[fast];
                slow.next = fast
                # slow++;
                slow = slow.next
            # fast++
            fast = fast.next
        # 断开与后面重复元素的连接
        slow.next = None
        return head


```



### <u>-- NSUM</u>

### [1. Two Sum](https://leetcode.com/problems/two-sum/) 💚

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly\* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

 

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 维护 val -> index 的映射
        valToIndex = {}
        for i in range(len(nums)):
            # 查表，看看是否有能和 nums[i] 凑出 target 的元素
            need = target - nums[i]
            if need in valToIndex:
                return [valToIndex[need], i]
            # 存入 val -> index 的映射
            valToIndex[nums[i]] = i
        return []
```



### [15. 3Sum](https://leetcode.com/problems/3sum/) 🧡

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

 #### Solution1 nsum的套路解法

```py
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

#### 3sum的优解

```py
def threeSum(self, nums: List[int]) -> List[List[int]]:

    if len(nums) < 3:
        return []

    res = set()

    # 1. Split nums into three lists: negative numbers, positive numbers, and zeros
    n, p, z = [], [], []
    for num in nums:
        if num > 0:
            p.append(num)
        elif num < 0:
            n.append(num)
        else:
            z.append(num)

    # 2. Create a separate set for negatives and positives for O(1) look-up times
    N, P = set(n), set(p)

    # 3. If there is at least 1 zero in the list, add all cases where -num exists in N and num exists in P
    #   i.e. (-3, 0, 3) = 0
    if z:
        for num in P:
            if -1 * num in N:
                res.add((-1 * num, 0, num))

        # 3. If there are at least 3 zeros in the list then also include (0, 0, 0) = 0
        if len(z) >= 3:
            res.add((0, 0, 0))

    # 4. For all pairs of negative numbers (-3, -1), check to see if their complement (4)
    #   exists in the positive number set
    from itertools import combinations

    for x, y in combinations(n, 2):
        target = -1 * (x + y)
        if target in P:
            res.add(tuple(sorted([x, y, target])))

    # 5. For all pairs of positive numbers (1, 1), check to see if their complement (-2)
    #   exists in the negative number set

    for x, y in combinations(p, 2):
        target = -1 * (x + y)
        if target in N:
            res.add(tuple(sorted([x, y, target])))

    return [list(x) for x in res]
```



```py
from typing import List

class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        def Nsum(nums: List[int], target: int, N: int, result: List[int], Nsums: List[List[int]]):
            if len(nums) < N or N < 2 or target < nums[0]*N or target > nums[-1]*N:
                return
            elif N == 2:
                j, k = 0, len(nums) - 1
                while j < k:
                    s = nums[k] + nums[j]
                    if s > target:
                        k -= 1
                    elif s < target:
                        j += 1
                    else:
                        Nsums.append(result + [nums[j], nums[k]])
                        j += 1
                        k -= 1
                        while nums[j-1] == nums[j] and j < k:
                            j += 1
                        while nums[k+1] == nums[k] and k > j:
                            k -= 1
            else:
                for i in range(len(nums)-N+1):
                    if i == 0 or (i > 0 and nums[i-1] != nums[i]):
                        Nsum(nums[i+1:], target-nums[i], N-1, result+[nums[i]], Nsums)
        
        nums.sort()
        foursums = []
        Nsum(nums, target, 4, [], foursums)
        return foursums
```



### <u>- Sliding Window (Fast and Slow pointer)</u>

> [!NOTE]
>
> **滑动窗口算法技巧主要用来解决子数组问题，比如让你寻找符合某个条件的最长/最短子数组**。
>
> **滑动窗口算法的思路是这样**：
>
> 1、我们在字符串 `S` 中使用双指针中的左右指针技巧，初始化 `left = right = 0`，把索引**左闭右开**区间 `[left, right)` 称为一个「窗口」。
>
> 

> [!TIP]
>
> 为什么要「左闭右开」区间
>
> **理论上你可以设计两端都开或者两端都闭的区间，但设计为左闭右开区间是最方便处理的**。
>
> 因为这样初始化 `left = right = 0` 时区间 `[0, 0)` 中没有元素，但只要让 `right` 向右移动（扩大）一位，区间 `[0, 1)` 就包含一个元素 `0` 了。
>
> 如果你设置为两端都开的区间，那么让 `right` 向右移动一位后开区间 `(0, 1)` 仍然没有元素；如果你设置为两端都闭的区间，那么初始区间 `[0, 0]` 就包含了一个元素。这两种情况都会给边界处理带来不必要的麻烦。

> [!NOTE]
>
> 2、我们先不断地增加 `right` 指针扩大窗口 `[left, right)`，直到窗口中的字符串符合要求（包含了 `T` 中的所有字符）。
>
> 3、此时，我们停止增加 `right`，转而不断增加 `left` 指针缩小窗口 `[left, right)`，直到窗口中的字符串不再符合要求（不包含 `T` 中的所有字符了）。同时，每次增加 `left`，我们都要更新一轮结果。
>
> 4、重复第 2 和第 3 步，直到 `right` 到达字符串 `S` 的尽头。
>
> 这个思路其实也不难，**第 2 步相当于在寻找一个「可行解」，然后第 3 步在优化这个「可行解」，最终找到最优解**，也就是最短的覆盖子串。左右指针轮流前进，窗口大小增增减减，就好像一条毛毛虫，一伸一缩，不断向右滑动，这就是「滑动窗口」这个名字的来历。

### [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) ❤️

Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window*** 

***substring\***

 *of* `s` *such that every character in* `t` *(**including duplicates**) is included in the window*. If there is no such substring, return *the empty string* `""`.

The testcases will be generated such that the answer is **unique**.

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

```py
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
```

### [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/) 🧡

Given two strings `s1` and `s2`, return `true` *if* `s2` *contains a permutation of* `s1`*, or* `false` *otherwise*.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**

```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```

```py
# 注意：python 代码由 chatGPT🤖 根据我的 cpp 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def checkInclusion(self, t: str, s: str) -> bool:
        # 创建字典，记录字符需要出现的次数
        need, window = Counter(t), Counter()
        left, right, valid = 0, 0, 0
        
        # 右指针前移，更新窗口内数据
        while right < len(s):
            c = s[right]
            right += 1
            # 如果该字符在需要的字典里，更新窗口内字典
            if need[c]:
                window[c] += 1
                # 如果窗口内字典该字符准确次数与需要的次数相同，计数器+1
                if window[c] == need[c]:
                    valid += 1

            # 判断左侧窗口是否需要收缩
            if right - left >= len(t):
                # 如果子串合法，返回True
                if valid == len(need):
                    return True
                # 左指针前移了，需要从窗口内字典中减掉一个元素
                d = s[left]
                left += 1
                if need[d]:
                    # 如果窗口内字典该字符准确次数与需要的次数相同，计数器-1
                    if window[d] == need[d]:
                        valid -= 1
                    window[d] -= 1
        # 未找到合法的子串，返回False
        return False
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=permutation-in-string

```





### [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) 🧡

Given two strings `s` and `p`, return *an array of all the start indices of* `p`*'s anagrams in* `s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```



```py
# 注意：python 代码由 chatGPT🤖 根据我的 cpp 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def findAnagrams(self, s: str, t: str) -> List[int]:
        need = {}
        window = {}
        for c in t:
            if c in need:
                need[c] += 1
            else:
                need[c] = 1

        left = 0
        right = 0
        valid = 0
        res = []
        
        while right < len(s):
            c = s[right]
            right += 1
            # 进行窗口内数据的一系列更新
            if c in need:
                if c in window:
                    window[c] += 1
                else:
                    window[c] = 1
                if window[c] == need[c]:
                    valid += 1
            # 判断左侧窗口是否要收缩
            while right - left >= len(t):
                # 当窗口符合条件时，把起始索引加入 res
                if valid == len(need):
                    res.append(left)
                d = s[left]
                left += 1
                # 进行窗口内数据的一系列更新
                if d in need:
                    if window[d] == need[d]:
                        valid -= 1
                    window[d] -= 1

        return res
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=find-all-anagrams-in-a-string

```

### [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) 🧡

Given a string `s`, find the length of the **longest**  **substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```



```py
# 注意：python 代码由 chatGPT🤖 根据我的 cpp 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        window = {}

        left = right = 0
        res = 0 # 记录结果
        while right < len(s):
            c = s[right]
            right += 1
            # 进行窗口内数据的一系列更新
            window[c] = window.get(c, 0) + 1
            # 判断左侧窗口是否要收缩
            while window[c] > 1:
                d = s[left]
                left += 1
                # 进行窗口内数据的一系列更新
                window[d] -= 1
            # 在这里更新答案
            res = max(res, right - left)
        return res
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=longest-substring-without-repeating-characters

```



### - Binary Search

```py
# LeeCode解法
# 二分查找 - Python代码

def binarySearch(nums:List[int], target:int) -> int:
    left = 0; 
    right = len(nums) - 1; # 注意

    while(left <= right):
        mid = left + (right - left) // 2; # 防止整型溢出
        if(nums[mid] == target):
            return mid; 
        elif(nums[mid] < target):
            left = mid + 1; # 注意
        elif(nums[mid] > target):
            right = mid - 1; # 注意

    return -1;

```



```py
def left_bound(nums: List[int], target: int) -> int:
    left = 0
    right = nums.length; # 注意
    
    while left < right: # 注意
        mid = left + (right - left) // 2
        if nums[mid] == target:
            right = mid
        elif nums[mid] < target:
            left = mid + 1
        elif nums[mid] > target:
            right = mid; # 注意

    return left

```

> [!NOTE]
>
> 当 `target` 不存在时，`left_bound` 返回值的含义
>
> 这是一个很好且很重要的问题，你把这个地方理解了，在二分搜索的实际应用场景中就不会懵逼。
>
> 直接说结论：**如果 `target` 不存在，搜索左侧边界的二分搜索返回的索引是大于 `target` 的最小索引**。

### [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) 🧡

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

```py
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        return [self.left_bound(nums, target), self.right_bound(nums, target)]
    
    def left_bound(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        # 搜索区间为 [left, right]
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] < target:
                # 搜索区间变为 [mid+1, right]
                left = mid + 1
            elif nums[mid] > target:
                # 搜索区间变为 [left, mid-1]
                right = mid - 1
            elif nums[mid] == target:
                # 收缩右侧边界
                right = mid - 1
        # 检查出界情况
        if left >= len(nums) or nums[left] != target: 

            return -1
        return left
    
    def right_bound(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            elif nums[mid] == target:
                # 这里改成收缩左侧边界即可
                left = mid + 1
        # 这里改为检查 right 越界的情况，见下图
        if right < 0 or nums[right] != target: 

            return -1
        return right
```



### [704. Binary Search](https://leetcode.com/problems/binary-search/) 💚

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

```py
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1  # 注意

        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1  # 注意
            elif nums[mid] > target:
                right = mid - 1  # 注意

        return -1

```

### -- 二分搜索问题的泛化

什么问题可以运用二分搜索算法技巧？

**首先，你要从题目中抽象出一个自变量 `x`，一个关于 `x` 的函数 `f(x)`，以及一个目标值 `target`**。

同时，`x, f(x), target` 还要满足以下条件：

**1、`f(x)` 必须是在 `x` 上的单调函数（单调增单调减都可以）**。

**2、题目是让你计算满足约束条件 `f(x) == target` 时的 `x` 的值**。



**1、确定 `x, f(x), target` 分别是什么，并写出函数 `f` 的代码**。

**2、找到 `x` 的取值范围作为二分搜索的搜索区间，初始化 `left` 和 `right` 变量**。

**3、根据题目的要求，确定应该使用搜索左侧还是搜索右侧的二分搜索算法，写出解法代码**。

### [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) 🧡

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return *the minimum integer* `k` *such that she can eat all the bananas within* `h` *hours*.

 

**Example 1:**

```
Input: piles = [3,6,7,11], h = 8
Output: 4
```

```py
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        max_speed=max(piles)
        min_speed=1
        ans=max_speed

        while min_speed<=max_speed:
            mid=(min_speed+max_speed)//2
            time=0
            for pile in piles:
                time+=math.ceil(pile/mid)
            if time<=h:
                ans=min(ans,mid)
                max_speed=mid-1
            else:
                min_speed=mid+1
        return ans
```



### [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) ❤️

Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.

Return *the minimized largest sum of the split*.

A **subarray** is a contiguous part of the array.

 

**Example 1:**

```
Input: nums = [7,2,5,10,8], k = 2
Output: 18
Explanation: There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.
```



```py
class Solution:
    def splitArray(self, nums: List[int], k: int) -> int:
        def check(arr, guess, k):
            total = 0
            count = 1
            for i in arr:
                if total + i > guess:
                    total = 0
                    count += 1
                total += i
            return count > k
                

        left = max(nums)
        right = sum(nums)

        while left<right:
            mid = (left+right)//2
            if check(nums, mid, k):
                left = mid + 1
            else:
                right = mid

        return left


```



### [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

[labuladong 题解](https://labuladong.online/algo/slug.html?slug=capacity-to-ship-packages-within-d-days)[思路](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/description/#)

Solved

Medium



Topics

Companies



Hint

A conveyor belt has packages that must be shipped from one port to another within `days` days.

The `ith` package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

 

**Example 1:**

```
Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
Output: 15
Explanation: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.
```



```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def shipWithinDays(self, weights: List[int], days: int) -> int:
        left = max(weights)
        right = sum(weights)

        while left < right:
            mid = (left + right) // 2
            if self.f(weights, mid) <= days:
                right = mid
            else:
                left = mid + 1

        return left


    def f(self, weights, x):
        days = 0
        i = 0
        while i < len(weights):
            # 尽可能多装货物
            cap = x
            while i < len(weights):
                if cap < weights[i]:
                    break
                else:
                    cap -= weights[i]
                    i += 1
            days += 1
        return days

```

### - PrefixSum

### [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) 💚

Given an integer array `nums`, handle multiple queries of the following type:

1. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

 

**Example 1:**

```
Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
```

```py
class NumArray:
    # 前缀和数组
    def __init__(self, nums: List[int]):
        self.preSum = [0] * (len(nums) + 1)
        for i in range(1, len(self.preSum)):
            self.preSum[i] = self.preSum[i - 1] + nums[i - 1]
    
    # 查询闭区间 [left, right] 的累加和
    def sumRange(self, left: int, right: int) -> int:
        return self.preSum[right + 1] - self.preSum[left]
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=range-sum-query-immutable

```



### [304. Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/) 🧡

Given a 2D matrix `matrix`, handle multiple queries of the following type:

- Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

Implement the `NumMatrix` class:

- `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
- `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

You must design an algorithm where `sumRegion` works on `O(1)` time complexity.

```py
class NumMatrix:
    # preSum[i][j] 记录矩阵 [0, 0, i, j] 的元素和
    def __init__(self, matrix: List[List[int]]):
        m, n = len(matrix), len(matrix[0])
        if m == 0 or n == 0:
            return
        # 构造前缀和矩阵
        self.preSum = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                # 计算每个矩阵 [0, 0, i, j] 的元素和
                self.preSum[i][j] = self.preSum[i - 1][j] + self.preSum[i][j - 1] + matrix[i - 1][j - 1] - self.preSum[i - 1][j - 1]

    # 计算子矩阵 [x1, y1, x2, y2] 的元素和
    def sumRegion(self, x1: int, y1: int, x2: int, y2: int) -> int:
        # 目标矩阵之和由四个相邻矩阵运算获得
        return self.preSum[x2 + 1][y2 + 1] - self.preSum[x1][y2 + 1] - self.preSum[x2 + 1][y1] + self.preSum[x1][y1]
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=range-sum-query-2d-immutable

```

### -Difference Array

**差分数组的主要适用场景是频繁对原始数组的某个区间的元素进行增减**。

比如说，我给你输入一个数组 `nums`，然后又要求给区间 `nums[2..6]` 全部加 1，再给 `nums[3..9]` 全部减 3，再给 `nums[0..4]` 全部加 2，再给...

一通操作猛如虎，然后问你，最后 `nums` 数组的值是什么？

常规的思路很容易，你让我给区间 `nums[i..j]` 加上 `val`，那我就一个 for 循环给它们都加上呗，还能咋样？这种思路的时间复杂度是 O(N)，由于这个场景下对 `nums` 的修改非常频繁，所以效率会很低下。

这里就需要差分数组的技巧，类似前缀和技巧构造的 `preSum` 数组，我们先对 `nums` 数组构造一个 `diff` 差分数组，**`diff[i]` 就是 `nums[i]` 和 `nums[i-1]` 之差**：

```py
class Difference:
    # 差分数组
    def __init__(self, nums: List[int]):
        assert len(nums) > 0
        self.diff = [0] * len(nums)
        # 根据初始数组构造差分数组
        self.diff[0] = nums[0]
        for i in range(1, len(nums)):
            self.diff[i] = nums[i] - nums[i - 1]

    # 给闭区间 [i, j] 增加 val（可以是负数）
    def increment(self, i: int, j: int, val: int) -> None:
        self.diff[i] += val
        if j + 1 < len(self.diff):
            self.diff[j + 1] -= val

    # 返回结果数组
    def result(self) -> List[int]:
        res = [0] * len(self.diff)
        # 根据差分数组构造结果数组
        res[0] = self.diff[0]
        for i in range(1, len(self.diff)):
            res[i] = res[i - 1] + self.diff[i]
        return res

```

### [1109. Corporate Flight Bookings](https://leetcode.com/problems/corporate-flight-bookings/) 🧡

There are `n` flights that are labeled from `1` to `n`.

You are given an array of flight bookings `bookings`, where `bookings[i] = [firsti, lasti, seatsi]` represents a booking for flights `firsti` through `lasti` (**inclusive**) with `seatsi` seats reserved for **each flight** in the range.

Return *an array* `answer` *of length* `n`*, where* `answer[i]` *is the total number of seats reserved for flight* `i`.

 

**Example 1:**

```
Input: bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
Output: [10,55,45,25,25]
Explanation:
Flight labels:        1   2   3   4   5
Booking 1 reserved:  10  10
Booking 2 reserved:      20  20
Booking 3 reserved:      25  25  25  25
Total seats:         10  55  45  25  25
Hence, answer = [10,55,45,25,25]
```

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。
class Difference:
        # 差分数组
    def __init__(self, nums: List[int]):
        assert len(nums) > 0
        self.diff = [0] * len(nums)
            # 构造差分数组
        self.diff[0] = nums[0]
        for i in range(1, len(nums)):
            self.diff[i] = nums[i] - nums[i - 1]

        # 给闭区间 [i, j] 增加 val（可以是负数）
    def increment(self, i: int, j: int, val: int) -> None:
        self.diff[i] += val
        if j + 1 < len(self.diff):
            self.diff[j + 1] -= val

    def result(self) -> List[int]:
        res = [0] * len(self.diff)
            # 根据差分数组构造结果数组
        res[0] = self.diff[0]
        for i in range(1, len(self.diff)):
            res[i] = res[i - 1] + self.diff[i]
        return res
        
class Solution:
    def corpFlightBookings(self, bookings: List[List[int]], n: int) -> List[int]:
        # nums 初始化为全 0
        nums = [0] * n
        # 构造差分解法
        df = Difference(nums)

        for booking in bookings:
            # 注意转成数组索引要减一哦
            i, j, val = booking[0] - 1, booking[1] - 1, booking[2]
            # 对区间 nums[i..j] 增加 val
            df.increment(i, j, val)
        # 返回最终的结果数组
        return df.result()


# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=corporate-flight-bookings

```

### [1094. Car Pooling](https://leetcode.com/problems/car-pooling/) 🧡

There is a car with `capacity` empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

You are given the integer `capacity` and an array `trips` where `trips[i] = [numPassengersi, fromi, toi]` indicates that the `ith` trip has `numPassengersi` passengers and the locations to pick them up and drop them off are `fromi` and `toi` respectively. The locations are given as the number of kilometers due east from the car's initial location.

Return `true` *if it is possible to pick up and drop off all passengers for all the given trips, or* `false` *otherwise*.

 

**Example 1:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false
```

```py
class Solution:
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        # 最多有 1000 个车站
        nums = [0] * 1001
        # 构造差分解法
        df = self.Difference(nums)

        for trip in trips:
            # 乘客数量
            val = trip[0]
            # 第 trip[1] 站乘客上车
            i = trip[1]
            # 第 trip[2] 站乘客已经下车，
            # 即乘客在车上的区间是 [trip[1], trip[2] - 1]
            j = trip[2] - 1
            # 进行区间操作
            df.increment(i, j, val)

        res = df.result()

        # 客车自始至终都不应该超载
        for i in range(len(res)):
            if capacity < res[i]:
                return False
        return True

    # 差分数组工具类
    class Difference:
        # 差分数组
        diff = []

        """输入一个初始数组，区间操作将在这个数组上进行"""
        def __init__(self, nums):
            assert len(nums) > 0
            self.diff = [0] * len(nums)
            # 根据初始数组构造差分数组
            self.diff[0] = nums[0]
            for i in range(1, len(nums)):
                self.diff[i] = nums[i] - nums[i - 1]

        """给闭区间 [i, j] 增加 val（可以是负数）"""
        def increment(self, i, j, val):
            self.diff[i] += val
            if j + 1 < len(self.diff):
                self.diff[j + 1] -= val

        """返回结果数组"""
        def result(self):
            res = [0] * len(self.diff)
            # 根据差分数组构造结果数组
            res[0] = self.diff[0]
            for i in range(1, len(self.diff)):
                res[i] = res[i - 1] + self.diff[i]
            return res
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=car-pooling

```

## Linked List

### - Double Pointer

1、合并两个有序链表

2、链表的分解

3、合并 `k` 个有序链表

4、寻找单链表的倒数第 `k` 个节点

5、寻找单链表的中点

6、判断单链表是否包含环并找出环起点

7、判断两个单链表是否相交并找出交点

### [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) 💚

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        # 虚拟头结点
        dummy = ListNode(-1)
        p = dummy
        p1 = l1
        p2 = l2

        while p1 and p2: 

            # 比较 p1 和 p2 两个指针
            # 将值较小的的节点接到 p 指针
            if p1.val > p2.val:
                p.next = p2
                p2 = p2.next
            else:
                p.next = p1
                p1 = p1.next
            # p 指针不断前进
            p = p.next

        if p1:
            p.next = p1

        if p2:
            p.next = p2

        return dummy.next
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=merge-two-sorted-lists

```



### [86. Partition List](https://leetcode.com/problems/partition-list/) 🧡

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

 

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
        
def partition(head: ListNode, x: int) -> ListNode:
    # 存放小于 x 的链表的虚拟头结点
    dummy1 = ListNode(-1)
    # 存放大于等于 x 的链表的虚拟头结点
    dummy2 = ListNode(-1)
    # p1, p2 指针负责生成结果链表
    p1, p2 = dummy1, dummy2
    # p 负责遍历原链表，类似合并两个有序链表的逻辑
    # 这里是将一个链表分解成两个链表
    p = head
    while p:
        if p.val >= x:
            p2.next = p
            p2 = p2.next
        else:
            p1.next = p
            p1 = p1.next
        # 不能直接让 p 指针前进，
        # p = p.next
        # 断开原链表中的每个节点的 next 指针
        temp = p.next
        p.next = None
        p = temp

    # 连接两个链表
    p1.next = dummy2.next

    return dummy1.next

```

> [!IMPORTANT]
>
> 总的来说，如果我们需要把原链表的节点接到新链表上，而不是 new 新节点来组成新链表的话，那么断开节点和原链表之间的链接可能是必要的。那其实我们可以养成一个好习惯，但凡遇到这种情况，就把原链表的节点断开，这样就不会出错了。
>
> ```
>         temp = p.next
>         p.next = None
>         p = temp
> ```
>
> 

### ?? [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) ❤️

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



```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        d=defaultdict(int)
        for i in lists:
            head=ListNode(0)
            head.next=i
            head=head.next
            while head:
                d[head.val]+=1
                head=head.next
        head=ListNode(0)
        cur=head
        val=sorted(d.keys(), reverse=True)
        while val:
            k=val.pop()
            for i in range(d[k]):
                cur.next=ListNode(k)
                cur=cur.next
        return head.next
```

优先队列 `pq` 中的元素个数最多是 `k`，所以一次 `poll` 或者 `add` 方法的时间复杂度是 `O(logk)`；所有的链表节点都会被加入和弹出 `pq`，**所以算法整体的时间复杂度是 `O(Nlogk)`，其中 `k` 是链表的条数，`N` 是这些链表的节点总数**。

### [19. Remove Nth Node From End of List ](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) 🧡

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        
        dummy_head =  ListNode(next = head)
        slow = fast = dummy_head
        #让双指针中一个指针领先几步的方法
        for i in range(n+1):#这里领先n+1步是因为slow指针需要操作的是倒数第n个节点前面一个节点，才能对倒数第n个节点进行删除操作
            fast = fast.next
        #两个指针同步后移
        while fast:
            slow = slow.next
            fast = fast.next

        slow.next = slow.next.next

        return dummy_head.next
```



### [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) 💚

Given the `head` of a singly linked list, return *the middle node of the linked list*.

If there are two middle nodes, return **the second middle** node.

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。

def middleNode(head: ListNode) -> ListNode:
    # 快慢指针初始化指向 head
    slow = head
    fast = head
    # 快指针走到末尾时停止
    while fast and fast.next:
        # 慢指针走一步，快指针走两步
        slow = slow.next
        fast = fast.next.next
    # 慢指针指向中点
    return slow

```



### [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) 💚

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` *if there is a cycle in the linked list*. Otherwise, return `false`.

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。

def hasCycle(head: ListNode) -> bool:
    # 快慢指针初始化指向 head
    slow, fast = head, head
    # 快指针走到末尾时停止
    while fast and fast.next:
        # 慢指针走一步，快指针走两步
        slow = slow.next
        fast = fast.next.next
        # 快慢指针相遇，说明含有环
        if slow == fast:
            return True
    # 不包含环
    return False
```

### [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) 🧡

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。

def detectCycle(head: ListNode) -> ListNode:
    fast, slow = head, head
    while fast and fast.next:
        fast = fast.next.next
        slow = slow.next
        if fast == slow:
            break 
    if not fast or not fast.next:
        return None
    slow = head 
    while slow != fast:
        fast = fast.next
        slow = slow.next
    return slow

```

### [160. Intersection of Two Linked Lists ](https://leetcode.com/problems/intersection-of-two-linked-lists/)💚

Given the heads of two singly linked-lists `headA` and `headB`, return *the node at which the two lists intersect*. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![img](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Custom Judge:**

The inputs to the **judge** are given as follows (your program is **not** given these inputs):

- `intersectVal` - The value of the node where the intersection occurs. This is `0` if there is no intersected node.
- `listA` - The first linked list.
- `listB` - The second linked list.
- `skipA` - The number of nodes to skip ahead in `listA` (starting from the head) to get to the intersected node.
- `skipB` - The number of nodes to skip ahead in `listB` (starting from the head) to get to the intersected node.

The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        # p1 指向 A 链表头结点，p2 指向 B 链表头结点
        p1, p2 = headA, headB
        while p1 != p2:
            # p1 走一步，如果走到 A 链表末尾，转到 B 链表
            if p1 is None:
                p1 = headB
            else:
                p1 = p1.next
            # p2 走一步，如果走到 B 链表末尾，转到 A 链表
            if p2 is None:
                p2 = headA
            else:
                p2 = p2.next
        return p1
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=intersection-of-two-linked-lists

```

### [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) 💚

Given the `head` of a singly linked list, return `true` *if it is a* 

*palindrome*

 *or* `false` *otherwise*.



```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        if fast:
            slow = slow.next

        left = head
        right = self.reverse(slow)
        while right:
            if left.val != right.val:
                return False
            left = left.next
            right = right.next
            
        return True
    
    def reverse(self, head: ListNode) -> ListNode:
        pre, cur = None, head
        while cur:
            next_node = cur.next
            cur.next = pre
            pre = cur
            cur = next_node
            
        return pre
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=palindrome-linked-list

```

 

### [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)❤️

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return *the modified list*.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        if not head:
            return None
        # 区间 [a, b) 包含 k 个待反转元素
        a = b = head
        for i in range(k):
            # 不足 k 个，不需要反转，base case
            if not b:
                return head
            b = b.next
        # 反转前 k 个元素
        newHead = self.reverse(a, b)
        # 递归反转后续链表并连接起来
        a.next = self.reverseKGroup(b, k) 

        return newHead

    """ 反转区间 [a, b) 的元素，注意是左闭右开 """ (迭代反转)
    def reverse(self, a: ListNode, b: ListNode) -> ListNode: 

        pre, cur, nxt = None, a, a
        # while 终止的条件改一下就行了
        while cur != b:
            nxt = cur.next
            cur.next = pre
            pre = cur
            cur = nxt
        # 返回反转后的头结点
        return pre
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=reverse-nodes-in-k-group

```



```py
# 定义：输入一个单链表头节点，将该链表反转，返回新的头结点（递归反转）
def reverse(head: ListNode) -> ListNode:
    if not head or not head.next:
        return head
    last = reverse(head.next) 
    head.next.next = head 
    head.next = None 
    return last

```

## Tree

**动归/DFS/回溯算法都可以看做二叉树问题的扩展，只是它们的关注点不同**：

- **动态规划算法属于分解问题的思路，它的关注点在整棵「子树」**。
- **回溯算法属于遍历的思路，它的关注点在节点间的「树枝」**。
- **DFS 算法属于遍历的思路，它的关注点在单个「节点」**。

### Traversal

先序、中序、后序其实指的是父节点被访问的次序。若在遍历过程中，父节点先于它的子节点被访问，就是先序遍历；父节点被访问的次序位于左右孩子节点之间，就是中序遍历；访问完左右孩子节点之后再访问父节点，就是后序遍历。不论是先序遍历、中序遍历还是后序遍历，左右孩子节点的相对访问次序是不变的，总是先访问左孩子节点，再访问右孩子节点。而层次遍历，就是按照从上到下、从左向右的顺序访问二叉树的每个节点。

#### preorderTraversal

前序遍历首先访问根节点，然后遍历左子树，最后遍历右子树。

```py
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def preorder(root):
    if not root:
        return 
    print(root.val)
    preorder(root.left)
    preorder(root.right)
    
def preorder(root):
    stack = [root]
    while stack:
        s = stack.pop()
        if s:
            print(s.val)
            stack.append(s.right)
            stack.append(s.left)

```



### [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) 💚

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

# 解法一，回溯算法思路

class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        self.res = 0
        self.traverse(root, 0)
        return self.res

    # 遍历二叉树
    def traverse(self, root: TreeNode, depth: int) -> None:
        if not root:
            return
        # 前序遍历位置
        depth += 1
        # 遍历的过程中记录最大深度
        self.res = max(self.res, depth)
        self.traverse(root.left, depth)
        self.traverse(root.right, depth)
        # 后序遍历位置
        depth -= 1


# 解法二，动态规划思路

class Solution:
    # 定义：输入一个节点，返回以该节点为根的二叉树的最大深度
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        leftMax = self.maxDepth(root.left)
        rightMax = self.maxDepth(root.right)
        # 根据左右子树的最大深度推出原二叉树的最大深度
        return 1 + max(leftMax, rightMax)
# 详细解析参见：
# https://labuladong.online/algo/slug.html?slug=maximum-depth-of-binary-tree

```

### [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/) 💚

Given the `root` of a binary tree, return *the preorder traversal of its nodes' values*.

```py

class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        
        res = []
        def dfs(currentNode, res):

            if not currentNode:
                return 
            res.append(currentNode.val)
            dfs(currentNode.left, res)
            dfs(currentNode.right, res)

        dfs(root, res)
        return res
    
```

### [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)💚

Given the `root` of a binary tree, invert the tree, and return *its root*.

 

```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:

        if root is None:
            return
        
        left = self.invertTree(root.right)
        right = self.invertTree(root.left)

        root.left = left
        root.right = right

        return root

        
```

> [!NOTE]
>
> 注意在class中调用method本身时，需要加self.。在Python中，当你在类的方法内部调用另一个方法时，需要使用`self`来引用当前对象的方法。

### [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/) 💚

Given a binary tree, determine if it is **height-balanced** 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

```py
class Solution(object):
    def isBalanced(self, root):
        return (self.Height(root) >= 0)
    def Height(self, root):
        if root is None:  
            return 0
        leftheight= self.Height(root.left)
        rightheight = self.Height(root.right)
        
        if leftheight < 0 or rightheight < 0 or abs(leftheight - rightheight) > 1:  
            return -1 #当某个子树不平衡时立马返回
          
        return max(leftheight, rightheight) + 1 ##跟子树相关所以需要设置返回值
```



### [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) 💚

Given the `root` of a binary tree, return *the length of the **diameter** of the tree*.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

class Solution:
    def __init__(self):
        self.maxDiameter = 0
        
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        self.maxDepth(root)
        return self.maxDiameter
    
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        leftMax = self.maxDepth(root.left)
        rightMax = self.maxDepth(root.right)
        # 后序遍历位置顺便计算最大直径
        self.maxDiameter = max(self.maxDiameter, leftMax + rightMax)
        return 1 + max(leftMax, rightMax)
 

```

> [!NOTE]
>
> 这里的最长直径不一定经过根节点

### [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)(BFS) 🧡

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right


class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        res = []
        if not root:
            return res

        q = []
        q.append(root)
        # while 循环控制从上向下一层层遍历
        while q:
            sz = len(q)
            # 记录这一层的节点值
            level = []
            # for 循环控制每一层从左向右遍历
            for i in range(sz):
                cur = q.pop(0)
                level.append(cur.val)
                if cur.left:
                    q.append(cur.left)
                if cur.right:
                    q.append(cur.right)
            res.append(level)
        return res
```



### ?? [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) 🧡

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码不保证正确性，仅供参考。如有疑惑，可以参照我写的 java 代码对比查看。

# 定义：在以 root 为根的二叉树中寻找值为 val 的节点
def find(root: TreeNode, val: int) -> TreeNode:
    # base case
    if not root:
        return None
    # 看看 root.val 是不是要找的
    if root.val == val:
        return root
    # root 不是目标节点，那就去左子树找
    left = find(root.left, val)
    if left:
        return left
    # 左子树找不着，那就去左子树找
    right = find(root.right, val)
    if right:
        return right
    # 实在找不到了
    return None

```

```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        # 在二叉树中寻找 val1 和 val2 的最近公共祖先节点
        def find(root, val1, val2):
            if not root:
                return None
            # 前序位置
            if root.val == val1 or root.val == val2:
                # 如果遇到目标值，直接返回
                return root
            left = find(root.left, val1, val2)
            right = find(root.right, val1, val2)
            # 后序位置，已经知道左右子树是否存在目标值
            if left and right:
                # 当前节点是 LCA 节点
                return root
            return left if left else right
        return find(root, p.val, q.val)

```

### [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) 🧡

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return *the values of the nodes you can see ordered from top to bottom*.

```py
# 注意：python 代码由 chatGPT🤖 根据我的 java 代码翻译，旨在帮助不同背景的读者理解算法逻辑。
# 本代码已经通过力扣的测试用例，应该可直接成功提交。

from collections import deque

class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        res = []
        if not root:
            return res
        # BFS 层序遍历，计算右侧视图
        q = deque()
        q.append(root)
        # while 循环控制从上向下一层层遍历
        while q:
            sz = len(q)
            # 每一层头部就是最右侧的元素
            last = q[0]
            for i in range(sz):
                cur = q.popleft()
                # 控制每一层从右向左遍历
                if cur.right:
                    q.append(cur.right)
                if cur.left:
                    q.append(cur.left)
            # 每一层的最后一个节点就是二叉树的右侧视图
            res.append(last.val)
        return res

    def rightSideView_DFS(self, root: TreeNode) -> List[int]:
        res = []
        # 记录递归的层数
        depth = 0
        def traverse(root: TreeNode):
            nonlocal depth
            if not root:
                return
            # 前序遍历位置
            depth += 1
            if len(res) < depth:
                # 这一层还没有记录值
                # 说明 root 就是右侧视图的第一个节点
                res.append(root.val)
            # 注意，这里反过来，先遍历右子树再遍历左子树
            # 这样首先遍历的一定是右侧节点
            traverse(root.right)
            traverse(root.left)
            # 后序遍历位置
            depth -= 1

        traverse(root)
        return res
```



### [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/) 🧡

You are given an integer array `nums` with no duplicates. A **maximum binary tree** can be built recursively from `nums` using the following algorithm:

1. Create a root node whose value is the maximum value in `nums`.
2. Recursively build the left subtree on the **subarray prefix** to the **left** of the maximum value.
3. Recursively build the right subtree on the **subarray suffix** to the **right** of the maximum value.

Return *the **maximum binary tree** built from* `nums`.

```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        
        max_index = 0
        max_num = 0
        for i in range(len(nums)):
            if nums[i] > max_num:
                max_num = nums[i]
                max_index = i
                
        root = TreeNode(max_num) 

        root.left = self.constructMaximumBinaryTree(nums[0:max_index])
        root.right = self.constructMaximumBinaryTree(nums[max_index+1:len(nums)])

        return root

        
```

