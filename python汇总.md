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

## 二、抽象类型和基本数据结构

1. 栈
   用list结构处理
   append（）添加
   pop（）出栈
2. 字典
   {}
   键和值
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

1. 比较
2. 排序
3. 扫描
4. 贪婪
5. 动态规划
6. 整数编码集合
7. 二分

## 四、字符串操作
