## Divide And Conquer:

**分治**

**"分而治之"( Divide and conquer)方法**(又称“分治术”) ，是有效算法设计中普遍采用的一种技术。
所谓“分而治之” 就是把一个复杂的算法问题按一定的“分解”方法分为等价的规模较小的若干部分，然后逐个解决，分别找出各部分的解，把各部分的解组成整个问题的解，这种朴素的思想来源于人们生活与工作的经验，也完全适合于技术领域。诸如软件的体系结构设计、模块化设计都是分而治之的具体表现。

分治（Divide and Conquer）是一种算法范式，也是一种解决问题的思想。

**步骤如下:**

> 1.分解（Divide）：将问题分解为同一类型的子问题；    
> 2.治理（Conquer）：递归地解决子问题；     
> 3.合并（Combine）：合并子问题的答案，得出原问题的答案。    

任何一个可以用计算机求解的问题，所需的计算时间都与其规模有关。问题的规模越小，越容易直接求解，所需的计算时间也越少。
例如，对于n各元素排序问题，当n=1时，不需要计算；n=2时，只需做一次比较；当n=3时，只需做3次比较……当n很大时，比较的次数是巨大的。
分治算法，就是把问题分解为同一性质的子问题，再讲子问题分解（递归），直到分解出的问题（最小子问题）可以直接求解。然后由这个解再一层层地回到原问题，同时在此过程中得到对应层的解。

 
**递归**

递归地两个要素：   
> 1.基线条件（Base Case）：循环调用的结束。也就是上面说的可以直接求解的“最小子问题”。     
> 2.递归条件（Recursive Case）：继续调用自己的条件。也就是将问题继续分解。

------------

## 分治的应用例子:

> 1.查找算法：二分法（Binary Search）      
> 2.排序算法：归并排序（Merge Sort）、快速排序（Quick Sort）
> 3.最接近点对问题（ Closest Pair of Points）     
> 4.Strassen矩阵乘法（ Strassen's Algorithm）    
> 5.傅里叶变换（ Cooley–Tukey Fast Fourier Transform (FFT) algorithm）     

### 1.查找算法：

#### Binary Search:

二分查找的步骤可总结如下：

> 如果 x 等于中间项，则退出。否则：

> 1）将数组划分为两个子数组，其大小约为原数组的一半。如果 x 小于中间项，则选择左子数组；如果 x 大于中间项，则选择右子数组。
> 2）确定 x 是否在该子数组中，以攻克（解决）该子数组。如果子数组不够小，则进行递归处理。
> 3）由子数组的答案获得原数组的答案。

二分查找是最简单的分而治之算法，因为原实例仅被分解为一个较小实例，所以不存在输出结果的组合。原实例的答案就是较小实例的答案。

```python
"""
Title : Binary Search
Created By Harrytsz.
"""

def binary_search(data, target, low, high):
    """
    Return : True if target is found in indicated portion of a Python list.
    """
    if low > high:
        return False
    else:
        mid = (low + high)//2
        if target == data[mid]:
            return True
        elif target < data[mid]:
            return binary_search(data, target, low, mid-1)
        else:
            return binary_search(data, target, mid+1, high)
```

--------

### 2.排序算法:

#### 1. Merge Sort:

**基于数组的归并排序(递归版)：**
```python
def merge_sort(S):
    """
    Recursion Version
    Created By Harrytsz.
    """
    n = len(S)
    if n < 2: return   # list is already sorted
    # divide
    mid = n // 2
    S1 = S[0:mid]     # Copy of first half
    S2 = S[mid:n]     # Copy of second half
    # conquer (with recursion)
    merge_sort(S1)
    merge_Sort(S2)
    # merge results
    merge(S1, S2, S)  # merge sorted halves back into S
    
def merge(S1, S2, S):
    """
    Merge two sorted Python list S1 and S2 into properly sized list S.
    """
    i = j = 0
    while i+j < len(S):
        if j == len(S2) or (i < len(S1) and S1[i] < S2[j]):
            S[i+j] = S1[i]
            i += 1
        else:
            S[i+j] = S2[j]
            j += 1
```
------------

**基于数组的归并排序(非递归版)：**

```python
def merge(src, res, start, inc):
    end1 = start + inc  # boundary of run 1
    end2 = min(start + 2*inc, len(src))   # boundary of run 2
    x, y, z = start, start+inc, start    # index into run 1, run 2, result
    while x < end1 and y < end2:
        if src[x] < src[y]:
            res[z] = src[x]; x += 1
        else:
            res[z] = src[y]; y += 1
        z += 1
    if x < end1:
        res[z:end2] = src[x:end1]
    elif y < end2:
        res[z:end2] = src[y:end2]
        
def merge_sort(S):
    """
    Sort the elements of Python list S using the merge_sort algorithm.
    """
    n = len(S)
    logn = math.ceil(math.log(n, 2))
    src, dest = S, [None]*n      # make temporary storage for dest
    for i in (2**k for k in range(logn)):
        for j in range(0, n, 2*i):
            merge(src, dest, j, i)
        src, dest = dest, src
    if S is not src:
        S[0:n] = src[0:n]
```

------------


**基于队列的归并排序：**

```python
def merge(S1, S2, S):
    """
    Merge two sorted queue instance S1 and S2 into empty queue S.
    """
    while not S1.is_empty() and not S2.is_empty():
        if S1.first() < S2.first():
            S.enqueue(S1.dequeue())
        else:
            S.enqueue(S2.dequeue())
    while not S1.is_empty():
        S.enqueue(S1.dequeue())
    while not S2.is_empty():
        S.enqueue(S2.dequeue())
        
def merge_sort(S):
    """
    Sort the elements of queue S using the merge_sort algorithm.
    """
    n = len(S)
    if n < 2: return            # list is already sorted.
    # divide 
    S1 = LinkedQueue()          # or any other queue implementation
    S2 = LinkedQueue()  
    while len(S1) < n // 2:     # move the first n//2 elements to S1
        S1.enqueue(S.dequeue())
    while not S.is_empty():
        S2.enqueue(S.dequeue())
    # conquer (with recursion)
    merge_sort(S1)
    merge_sort(S2)
    # merge results
    merge(S1, S2, S)            # merge sorted halves back into S
```

-------

#### 2. Quick Sort:

```python
  def quick_sort(array):
 
    if len(array) < 2:  # 基线条件——最小子问题：数组中只有1个或0个数字，则无需再排序
        return array
    else：                  # 递归条件——继续分解
        pivot = array[0]
        less = [i for i in array[1:] if i <= pivot]
        greater = [i for i in array[1:] if i > pivot]
        return quick_sort(less) + [pivot] + quick_sort(greater)
```
