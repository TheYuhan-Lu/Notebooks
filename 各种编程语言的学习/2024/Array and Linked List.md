# Array

##  Features   

- Stored in a continuous space
- Elements cannot be deleted directly, but **can only be replaced**.
- The element in the array can be found via the index, and the index is started with 0, which means that the first element in the array is `array[0]`



## Algorithm for Arrays

### Binary search

**There are two other Binary Search which could find the leftmost one and the rightmost one when the target number appears more than once**

Binary search only useful for ordered sequences. 

#### LeetCode 704

```python3

#Basic Binary Search 
#Time Complexity：O(log n)
#Space Complexity：O(1)
class Solution:
    def search(self, nums: list[int], target: int) -> int: #self shows that this function can use as a function for Solution.search
       left = 0
       right = len(nums)-1
       while left <= right:
       mid = (left + right)//2
       
       if nums[mid]==target:
           return mid
       elif nums[mid]< target:
           left = mid+1
       elif nums[mid] > target:
           right = mid-1

     return -1  #made a mistake here
  

```

### Double Pointer

#### LeetCode 27 Remove Element form Array

- Binary Search Solution

```python
def removeElement(nums: list[int], val: int) -> int:
    #solution using the basic binary search
    nums.sort()
    left = 0
    right = len(nums)
		while left < right:
    	mid = left + (right - left)//2
    	if nums[mid] == val:
        	right = mid
    	elif nums[mid] < val:
        	left = mid + 1
    	elif nums[mid] > val:
        	right = mid
     
    while left < len(nums) and nums[left] == val:
    		del nums[left]
		
    print(nums)
    return len(nums)
  

 removeElement([3,2,2,3,4],3)    
```

- Double Pointer

  There are two kinds of double pointer:

  - Left and Right Pointers (directional side)
  - Fast and Slow Pointers (same side)

```python3
def removeElement(nums: list[int], val: int) -> int:

```

#### LeetCode 977 Square of sorted array 

- In python, when you assign one list with another list, if the list is changed, so the original list element will also be changed. So typically do not use one list to assign a list if you still need the element in the original list.

  - Solution 1 --- Double Pointer

  ```python
  class Solution:
      def sortedSquares(self, nums: list[int]) -> list[int]:
  # using double pointer to solve it with O(n)
      Square_nums = [0] * len(nums) # notice the changable nums list can not use directly to define a new list here
      i,j,k = 0, len(nums)-1, len(nums)-1
      while i <= j:
          if nums[i]**2 > nums[j]**2:
              Square_nums[k]= nums[i]**2
              k = k-1
              i = i+1
          else:
              Square_nums[k]= nums[j]**2
              k =k - 1
              j = j - 1
  
       return Square_nums
  ```

   - Solution 2 --- Normal Solution

  ```python
  class Solution:
      def sortedSquares(self, nums: list[int]) -> list[int]:
          #normal solution with O(nlogn)
          Square_nums=nums
          for i in range(0,len(nums),1):
              Square_nums[i]=nums[i]**2
      return sorted(Square_nums)
  ```

  

### Sliding Window

#### LeetCode 209 Minimun Size Subarray Sum

Given an array of positive integers nums and a positive integer target, return the minimal length of a 
subarray

whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.



```python
class Solution:
    def minSubArrayLen(self, target: int, nums: list[int]) -> int:
        l = len(nums)
        left = 0
        right = 0
        cur_sum = 0
        min_len = float('inf') #表达一个正无穷的浮点数，在设置需要比较得到的最小值时可以采用，因为这样的正无穷对比较没有影响
    while right < l:
        cur_sum += nums[right] 

        while cur_sum >= target:
            min_len = min(min_len,(right-left)+1) 
            cur_sum -= nums[left]   #缩小窗口
            left += 1
        
        right += 1 #滑动右边界

    return min_len if min_len != float('inf') else 0 #return 中if语句的使用
```

### Spiral Matrix II

Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.

这种螺旋数组一是在按边走的时候，需要注意统一的开闭（即是否算入边界点）

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
       element = 1
       startx,starty = 0,0
       nums = [[0]*n for _ in range(n)]
       mid = n//2
       ori_n=n
   		 while n >= mid-1:

       		for y in range(starty,n-1,1):
           		nums[startx][y] = element
           		element += 1
        
       		for x in range(startx,n-1,1):
           		nums[x][n-1]= element
          		element += 1
        
       		for y in range(n-1,starty,-1):
           		nums[n-1][y] = element
           		element += 1

       		for x in range(n-1,startx,-1):
           		nums[x][starty]= element
           		element += 1     
        
       		n -= 1
       		startx += 1         # 更新起始点
       		starty += 1

   		 if ori_n % 2 != 0 :			# n为奇数时，填充中心点
        	nums[mid][mid] = element 

   		 return nums              
```




```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        nums = [[0] * n for _ in range(n)]
        startx, starty = 0, 0               # 起始点
        loop, mid = n // 2, n // 2          # 迭代次数、n为奇数时，矩阵的中心点
        count = 1                           # 计数
    		for offset in range(1, loop + 1) :      # 每循环一层偏移量加1，偏移量从1开始
        		for i in range(starty, n - offset) :    # 从左至右，左闭右开
            		nums[startx][i] = count
            		count += 1
        		for i in range(startx, n - offset) :    # 从上至下
            		nums[i][n - offset] = count
            		count += 1
        		for i in range(n - offset, starty, -1) : # 从右至左
            		nums[n - offset][i] = count
            		count += 1
        		for i in range(n - offset, startx, -1) : # 从下至上
            		nums[i][starty] = count
            		count += 1                
        		startx += 1         # 更新起始点
        		starty += 1

    		if n % 2 != 0 :			# n为奇数时，填充中心点
        		nums[mid][mid] = count 
    		return nums
```



# Linked List

## Types and Features

There are three types of **Linked List **:
- singly linked list:
  
  Every node in the singly linked list is made of two parts, one part is the date that stored in this node, and the another part is the pointer which point the next node. And the last node's second part is a null pointer(point Null).
  
- doubly linked list:

  Doubly linked list have a extra pointer which point the last node. So that the doubly linked list allows forward traversal and backward traversal.
  
- Circular Linked List



### Dummy Head

#### LeetCode 203  Remove Linked List Elements

Try to create a dummy head

```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy_head = ListNode(next = head)
        cur_node = dummy_head
        while cur_node.next:
            if cur_node.next.val == val:
                cur_node.next = cur_node.next.next
            else:
                cur_node = cur_node.next
        return dummy_head.next
```

#### LeetCode 206 [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

```python
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
            temp = cur.next
            cur.next = pre
            pre = cur
            cur = temp
       
        return pre
```

#### [LeetCode 24 Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        #need dummy head
        dummy_head = ListNode(next = head)
        cur = dummy_head
        while cur.next and cur.next.next:
            temp = cur.next
            temp1 = cur.next.next.next
            cur.next = cur.next.next
            cur.next.next = temp
            temp.next = temp1 
            cur = temp
        return dummy_head.next      
        
```

### Double Pointer ( Fast and Slow)

#### [LeetCode 19 Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

```python
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

#### [LeetCode 160 Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        cur_a, cur_b = headA, headB
        len_a,len_b = 0,0
        while cur_a:
            cur_a = cur_a.next
            len_a += 1
        #这道题的相交不是指有重叠部分，二是相交之后所有的节点都相同，合二为一了，所以说可以用这个找到同种长度的方法
        while cur_b:
            cur_b = cur_b.next
            len_b +=1

        cur_a, cur_b = headA, headB
        if len_a > len_b:
            cur_b, cur_a = headA, headB
            len_b, len_a = len_a, len_b

        for _ in range(len_b-len_a):
            cur_b = cur_b.next

        while len_a:
            if cur_b == cur_a:
                return cur_b
            cur_b,cur_a = cur_b.next,cur_a.next
        
        return null

```

