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
4. 并查集
