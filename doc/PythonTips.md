# Python3使用技巧

## 语法特性

#### 数组初始化

```python
# 初始化一个长度为 N 的一维数组
Array = [0] * N

# 初始化一个形状为 MxN 的二维数组(矩阵)
Matrix = [[0] * N for _ in range(M)] # 不可以写成 [[0] * N] * M，因为等号是复制，而非深拷贝。错误写法会导致在Matrix元素发生更改时，其他行对应的元素同步完成修改。
```

#### 交换元素值

```python
# c语言风格的交换两个元素值
tmp = a
a = b
b = tmp

# python风格
a, b = b, a
```

#### 连续不等式或等式

```python
# 判断 a，b，c 是否相等，Python里可以直接写连等
if a == b == c:
    return True

# 不等式也可以
if a <= b < c:
    return True
```

## 自带算法

#### 排序

Python自带的排序方法底层是基于Timsort实现的，Timsort结合了归并排序与插入排序。最优时间复杂度为O(n),最差时间复杂度为O(nlog(n)),平均时间复杂度同为O(nlog(n)),空间复杂度为O(n),并且是稳定排序。

|          | 接受参数     | 作用                 |
| -------- | ------------ | -------------------- |
| sorted() | reverse，key | 返回**新的**有序列表 |
| .sort()  | reverse，key | 将**原始**列表排序   |

#### 二分查找与插入

二分查找与插入可以考虑使用Python自带的bisect库实现。

⚠ 需要注意的是，在使用bisect库之前，需要保证列表是**有序**的。

| 函数                             | 说明                                                         | 时间复杂度 |
| -------------------------------- | ------------------------------------------------------------ | ---------- |
| bisect.bisect_left(data, value)  | 返回在data中插入value的位置，如果value已经在data里存在，那么插入点会在已存在元素之前 (也就是左边)。 | O(log(n))  |
| bisect.bisect_right(data, value) | 返回在data中插入value的位置，如果value已经在data里存在，那么插入点会在已存在元素之后 (也就是后边)。 | O(log(n))  |
| bisect.insort_left(data, value)  | 把value插入到data中已存在元素value的左侧。                   | O(n)       |
| bisect.insort_right(data, value) | 把value插入到data中已存在元素value的右侧。                   | O(n)       |

## 自带数据结构

Python自身已经实现了大部分典型的数据结构，如下表所示。除此之外，Python的collections库还提供了其他拓展的数据结构。

| 数据结构  | Python实现                  | 常用操作                                                     | 参考资料                                                     |
| --------- | --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 栈        | list类                      | append(value)、pop()                                         | [官方文档](https://docs.python.org/3/tutorial/datastructures.html#using-lists-as-stacks) |
| 队列      | collections.deque类（双向） | append(value)、appendleft(value)、pop()、popleft()           | [官方文档](https://docs.python.org/3/library/collections.html#collections.deque) |
| 堆        | list类 + heapq堆算法        | 构建满足规则的堆heapify(heap)、插入堆heappush(heap, value)、弹出堆heappop(heap)、弹出并插入堆heapreplace(heap, value)、返回n个最大值nlargest(n, heap)、返回n个最小值nsmallest(n, heap) | [官方文档](https://docs.python.org/3.8/library/heapq.html)   |
| HashSet   | set类                       | set(item), 交集（&），并集（\|），差集（set1 - set2）        | [官方文档](https://docs.python.org/3.8/library/stdtypes.html#set-types-set-frozenset) |
| HashTable | dict类                      | 返回指定键的值get(key)，update(dic2)，返回所有的键keys(), 返回所有的值values(), items()，创建一个新字典，以序列 seq 中元素做字典的键，value 为字典所有键对应的初始值。dict.fromkeys(seq, value) | [官方文档](https://docs.python.org/3/library/stdtypes.html#typesmapping) |

