# python 汇总

## 一、基础

1. python中表示无穷使用

   ```python
   float('inf')
   float('-inf')
   ```
2. 数组的复制

   ```python
   A = [1,2,3]
   # 完整复制
   B = A[:}
   # 去除首元素复制
   B = A[1:]
   # 去除末尾元素复制
   B = A[:-1]
   # 逆序复制
   B = A[::-1]

   ```
3. 矩阵的初始化

   ```python
   M1 = [[0]*10 for _ in range(10)]
   ```
4. 逆序处理数组

   ```python
   for i in range(9,-1,-1):
   ```
5. 数组切片

   ```python
   a[:]           # a copy of the whole array
   a[start:]      # items start through the rest of the array
   a[:stop]       # items from the beginning through stop-1
   a[start:stop]  # items start through stop-1

   a[start:stop:step] # start through not past stop, by step

   a[-1]    # last item in the array
   a[-2:]   # last two items in the array
   a[:-2]   # everything except the last two items


   a[::-1]    # all items in the array, reversed
   a[1::-1]   # the first two items, reversed
   a[:-3:-1]  # the last two items, reversed
   a[-3::-1]  # everything except the last two items, reversed

   ```

## 二、抽象类型和基本数据结构

1. 栈
   用list结构处理
   append（）添加
   pop（）出栈
2. 字典
   {}
   键和值
   可以用in 和 not in来判断是否在这里面。
3. 队列
   利用deque类（双向队列）
   append（）添加，popleft（）头部提取
   appendleft（）头部添加，pop（）尾部提取
4. 堆
   利用headq实现，可以实现添加数组并且取出数组中最大（小）的数字
   将数组转换为堆
   ```python
   heapify(table)
   # 添加元素
   heappush(heap,element)
   # 抽出最小元素
   heappop(heap)
   ```
5. 并查集

## 三、基础技术

### 1. 比较

使用字典序。

可以用字典去统计每个元素的出现次数，然后去返回最大值和其下标。

```python
max((tab[i],i) for i in range(len(tab)))

# 例如
def majority(L):
	compute = {}
	for word in L:
		if word in compute:
			compute[word] += 1
		else:
			compute[word] = 1
	valmin,argmin = min((-compute[word],word) for word in compute)
	return valmin,argmin
```

### 2. 排序

用sort（）或者sorted（）。

前者直接修改，后者会反悔相关列表一个排好序的副本。可以配合比较一起使用。

```python
list.sort(cmp=None, key=None, reverse=False)

# 用法
cmp -- 可选参数, 如果指定了该参数会使用该参数的方法进行排序。
key -- 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。
reverse -- 排序规则，reverse = True 降序， reverse = False 升序（默认）。

# 使用
## 获取列表的第二个元素
def takeSecond(elem):
    return elem[1]

## 列表
random = [(2, 2), (3, 4), (4, 1), (1, 3)]

## 指定第二个元素排序
random.sort(key=takeSecond)

## 输出类别
print('排序列表：')
print(random)

## 输出
排序列表：
[(4, 1), (2, 2), (1, 3), (3, 4)]
```

### 3. 前缀和

前缀和可以简单地理解为数组的前 i 个元素的和，当然其具体可以应用在一维以及二维的数组中：

* 快速求数组前 i 项之和
* 快速求数组的 [i,j] 范围内的和
* 快速求二维矩阵中某个子矩阵之和

```python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        n = len(nums)
        psum = [0]*(n+1)
        cur_sum = 0
        for i in range(n):
            cur_sum+=nums[i]
            psum[i+1]=cur_sum
        return psum
```

前缀和的前缀和数组创立，时间复杂度降为 O(n)。注意这里psum的第一项填充0，是为了方便后面区间和的求解统一。

区间和的求法可以建立在前缀和基础上。

对于前缀和数组的任一项有：`psum[i] = psum[i-1] + nums[i-1]`，此时只需谨记前缀和数组的下标与原数组下标不是直接对应，而是加一对应。即 psum[i] 表示 nums 前 i-1 项的，不包含当前元素 nums[i]。

那么，对于任意 i<=j<len(nums) ，数组 [i,j] 区间和公式：

```python
sum(i, j) = psum[j + 1] - psum[i]
```

 何时使用前缀和算法？

当题目中涉及 **区间和** ，前缀和为首选解决方案，子数组即为数组的区间表示。通常对于连续子数组的处理为双指针和滑动窗口，但当没有明确的指针移动判断条件时，可以尝试使用前缀和算法。

拓展学习：https://www.cnblogs.com/ZghzzZyu/p/16407303.html

### 4. 二分

建立中间值。

两种书写方法：

```python
#左闭右闭
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1  # 定义target在左闭右闭的区间里，[left, right]

        while left <= right:
            middle = left + (right - left) // 2

            if nums[middle] > target:
                right = middle - 1  # target在左区间，所以[left, middle - 1]
            elif nums[middle] < target:
                left = middle + 1  # target在右区间，所以[middle + 1, right]
            else:
                return middle  # 数组中找到目标值，直接返回下标
        return -1  # 未找到目标值

# 左闭右开
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)  # 定义target在左闭右开的区间里，即：[left, right)

        while left < right:  # 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
            middle = left + (right - left) // 2

            if nums[middle] > target:
                right = middle  # target 在左区间，在[left, middle)中
            elif nums[middle] < target:
                left = middle + 1  # target 在右区间，在[middle + 1, right)中
            else:
                return middle  # 数组中找到目标值，直接返回下标
        return -1  # 未找到目标值
```

### 5.. 扫描

从左到右地遍历输入元素，并对每个遇到的元素进行特定处理。

### 6. 贪婪

在寻找解决方案的每个步骤中都选择了一个让局部结果最大化的参数

### 7. 动态规划

把问题分解为若干子问题，并基于子问题的解决方案找到原始问题的最优解。

## 四、字符串操作


## 五、链表

定义链表：

```python
class ListNode:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
```

用python删除链表的一个元素不需要清空内存。

### 1. 链表操作

删除链表元素两种方式：

* **直接使用原来的链表来进行删除操作。**
* **设置一个虚拟头结点在进行删除操作。**

前一种方法需要重新单独为head写一个删除的。后一个方法**可以设置一个虚拟头结点** ，这样原链表的所有节点就都可以按照统一的方式进行移除了。

```python
（版本一）虚拟头节点法
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # 创建虚拟头部节点以简化删除过程
        dummy_head = ListNode(next = head)
    
        # 遍历列表并删除值为val的节点
        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
    
        return dummy_head.next
```



* get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
* addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
* addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
* addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
* deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

```python
（版本一）单链表法
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
    
class MyLinkedList:
    def __init__(self):
        self.dummy_head = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
    
        current = self.dummy_head.next
        for i in range(index):
            current = current.next
        
        return current.val

    def addAtHead(self, val: int) -> None:
        self.dummy_head.next = ListNode(val, self.dummy_head.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        current = self.dummy_head
        while current.next:
            current = current.next
        current.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
    
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = ListNode(val, current.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
    
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = current.next.next
        self.size -= 1


# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)

```

双链表法

```python
（版本二）双链表法
class ListNode:
    def __init__(self, val=0, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next

class MyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
    
        if index < self.size // 2:
            current = self.head
            for i in range(index):
                current = current.next
        else:
            current = self.tail
            for i in range(self.size - index - 1):
                current = current.prev
            
        return current.val

    def addAtHead(self, val: int) -> None:
        new_node = ListNode(val, None, self.head)
        if self.head:
            self.head.prev = new_node
        else:
            self.tail = new_node
        self.head = new_node
        self.size += 1

    def addAtTail(self, val: int) -> None:
        new_node = ListNode(val, self.tail, None)
        if self.tail:
            self.tail.next = new_node
        else:
            self.head = new_node
        self.tail = new_node
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
    
        if index == 0:
            self.addAtHead(val)
        elif index == self.size:
            self.addAtTail(val)
        else:
            if index < self.size // 2:
                current = self.head
                for i in range(index - 1):
                    current = current.next
            else:
                current = self.tail
                for i in range(self.size - index):
                    current = current.prev
            new_node = ListNode(val, current, current.next)
            current.next.prev = new_node
            current.next = new_node
            self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
    
        if index == 0:
            self.head = self.head.next
            if self.head:
                self.head.prev = None
            else:
                self.tail = None
        elif index == self.size - 1:
            self.tail = self.tail.prev
            if self.tail:
                self.tail.next = None
            else:
                self.head = None
        else:
            if index < self.size // 2:
                current = self.head
                for i in range(index):
                    current = current.next
            else:
                current = self.tail
                for i in range(self.size - index - 1):
                    current = current.prev
            current.prev.next = current.next
            current.next.prev = current.prev
        self.size -= 1



# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```
