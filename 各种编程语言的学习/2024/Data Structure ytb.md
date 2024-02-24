## Introduction to Data Structure

When we talk about DS

- mathematical/ logical models (**Abstract  Data Type**)
  - List
    - Store a given number of elements of any type
    - read elements by position
    - Modify element at a position
- Implementation
  - Arrays -- Concrete implementation![截屏2024-02-02 15.43.37](/Users/mercury/Library/Application Support/typora-user-images/截屏2024-02-02 15.43.37.png)





## Linked List

![image-20240205132358688](/Users/mercury/Library/Application Support/typora-user-images/image-20240205132358688.png)

### Array VS Linked List

1. Cost of accesing an element : Array is better than Linked List

![image-20240205133013075](/Users/mercury/Library/Application Support/typora-user-images/image-20240205133013075.png)

![image-20240205133409834](/Users/mercury/Library/Application Support/typora-user-images/image-20240205133409834.png)

![image-20240205133646072](/Users/mercury/Library/Application Support/typora-user-images/image-20240205133646072.png)

### Basic operation of Linked List

- Insert a node at the beginning of a linked list

- Insert a node at n th  position

- delete a node at n th position

- reverse a linked list

  ```py
  # Iteration
  def reverse(list: Optional[ListNode]-> Optional[ListNode]):
    cur = list
    new = None
    temp = list
    while cur:
    	temp = cur.next
      cur.next = new
      new = cur
      cur = temp
       
     return new
  
  ```
  
- print  a linked list using recursion

  ```py
  def print_linkedlist(head: Optional[ListNode]):
    
    print(head.val)
    while head.next:
    	print_linkedlist(head.next)
  ```
  

## Stack（ADT）

> [!NOTE]
>
> Last in First out

![image-20240206111450583](/Users/mercury/Library/Application Support/typora-user-images/image-20240206111450583.png)

![image-20240206111950733](/Users/mercury/Library/Application Support/typora-user-images/image-20240206111950733.png)

### Array implementation

```py

```

### Linked list implementation

```py

```

### Using stack to reverse

```py
## reverse a string
str = "HELLO"
stack = []
for i in range(0,len(str)-2)
  stack.append(c) 
  
for i in range(0,len(str)-2):
  str[i]= stack.top()
  stack.pop()
  
  
## reverse a linked list
push the address into the stack 

```

### Infix Postfix,prefix

#### Infix

![image-20240207101205867](/Users/mercury/Library/Application Support/typora-user-images/image-20240207101205867.png)

![image-20240207101544625](/Users/mercury/Library/Application Support/typora-user-images/image-20240207101544625.png)

#### Prefix

![image-20240207101644453](/Users/mercury/Library/Application Support/typora-user-images/image-20240207101644453.png)

#### Postfix

![image-20240207101825128](/Users/mercury/Library/Application Support/typora-user-images/image-20240207101825128.png)

![image-20240207101939131](/Users/mercury/Library/Application Support/typora-user-images/image-20240207101939131.png)

#### Evaluation of prefix and postfix expressions

![image-20240207102717696](/Users/mercury/Library/Application Support/typora-user-images/image-20240207102717696.png)

Scan from left to right for postfix

![image-20240207103031379](/Users/mercury/Library/Application Support/typora-user-images/image-20240207103031379.png)

Scan from right to left for prefix

![image-20240207103326017](/Users/mercury/Library/Application Support/typora-user-images/image-20240207103326017.png)

#### Infix to Postfix

Without the parentheses

![image-20240207104524201](/Users/mercury/Library/Application Support/typora-user-images/image-20240207104524201.png)

![image-20240207104542693](/Users/mercury/Library/Application Support/typora-user-images/image-20240207104542693.png)

```

```

With the parentheses

```

```



## Queues (ADT)

> [!NOTE]
>
> First In First Out

![image-20240207140112128](/Users/mercury/Library/Application Support/typora-user-images/image-20240207140112128.png)

![image-20240207140441664](/Users/mercury/Library/Application Support/typora-user-images/image-20240207140441664.png)

![image-20240207140623404](/Users/mercury/Library/Application Support/typora-user-images/image-20240207140623404.png)

### Implementation of Queues

![image-20240207140819148](/Users/mercury/Library/Application Support/typora-user-images/image-20240207140819148.png)

#### Array implementation

```py
# front and rear index
```

#### <u>circled array</u>

```py
i = (i+1)% len(array) #how using index to implement circled array
```

#### Linked list implementation

![image-20240207142521620](/Users/mercury/Library/Application Support/typora-user-images/image-20240207142521620.png)

![image-20240207142758221](/Users/mercury/Library/Application Support/typora-user-images/image-20240207142758221.png)

![image-20240207142952611](/Users/mercury/Library/Application Support/typora-user-images/image-20240207142952611.png)

## Graphs

### Introduction

![image-20240208100323224](/Users/mercury/Library/Application Support/typora-user-images/image-20240208100323224.png)


![image-20240208100356423](/Users/mercury/Library/Application Support/typora-user-images/image-20240208100356423.png)

![image-20240208100706725](/Users/mercury/Library/Application Support/typora-user-images/image-20240208100706725.png)

![image-20240208100833356](/Users/mercury/Library/Application Support/typora-user-images/image-20240208100833356.png)

![image-20240208100919662](/Users/mercury/Library/Application Support/typora-user-images/image-20240208100919662.png)

### Properties of Graphs

![image-20240208101051179](/Users/mercury/Library/Application Support/typora-user-images/image-20240208101051179.png)

![image-20240208101319020](/Users/mercury/Library/Application Support/typora-user-images/image-20240208101319020.png)

#### Number of edges

![image-20240208101547064](/Users/mercury/Library/Application Support/typora-user-images/image-20240208101547064.png)

#### Path

![image-20240208101706920](/Users/mercury/Library/Application Support/typora-user-images/image-20240208101706920.png)

(Path always refer to simple path)

![image-20240208101811105](/Users/mercury/Library/Application Support/typora-user-images/image-20240208101811105.png)

![image-20240208101959350](/Users/mercury/Library/Application Support/typora-user-images/image-20240208101959350.png)

![image-20240208102047060](/Users/mercury/Library/Application Support/typora-user-images/image-20240208102047060.png)

### Graph Representation

#### Edge List

![image-20240208102353157](/Users/mercury/Library/Application Support/typora-user-images/image-20240208102353157.png)

![image-20240208102509721](/Users/mercury/Library/Application Support/typora-user-images/image-20240208102509721.png)

![image-20240208103020705](/Users/mercury/Library/Application Support/typora-user-images/image-20240208103020705.png)

![image-20240208103151828](/Users/mercury/Library/Application Support/typora-user-images/image-20240208103151828.png)

#### Adjacency Matrix

![image-20240208103656540](/Users/mercury/Library/Application Support/typora-user-images/image-20240208103656540.png)

![image-20240208103725322](/Users/mercury/Library/Application Support/typora-user-images/image-20240208103725322.png)

![image-20240208104258189](/Users/mercury/Library/Application Support/typora-user-images/image-20240208104258189.png)

#### Adjacency List

![image-20240208105122399](/Users/mercury/Library/Application Support/typora-user-images/image-20240208105122399.png)

![image-20240208105350998](/Users/mercury/Library/Application Support/typora-user-images/image-20240208105350998.png)

