## Divide And Conquer:


"分而治之"( Divide and conquer)方法(又称“分治术”) ，是有效算法设计中普遍采用的一种技术。
所谓“分而治之” 就是把一个复杂的算法问题按一定的“分解”方法分为等价的规模较小的若干部分，然后逐个解决，分别找出各部分的解，把各部分的解组成整个问题的解，这种朴素的思想来源于人们生活与工作的经验，也完全适合于技术领域。诸如软件的体系结构设计、模块化设计都是分而治之的具体表现。

分治

分治（Divide and Conquer）是一种算法范式，也是一种解决问题的思想。

步骤如下：

> 1.分解（Divide）：将问题分解为同一类型的子问题；    
> 2.治理（Conquer）：递归地解决子问题；     
> 3.合并（Combine）：合并子问题的答案，得出原问题的答案。    

任何一个可以用计算机求解的问题，所需的计算时间都与其规模有关。
问题的规模越小，越容易直接求解，所需的计算时间也越少。
例如，对于n各元素排序问题，当n=1时，不需要计算；n=2时，只需做一次比较；当n=3时，只需做3次比较……当n很大时，比较的次数是巨大的。
分治算法，就是把问题分解为同一性质的子问题，再讲子问题分解（递归），直到分解出的问题（最小子问题）可以直接求解。然后由这个解再一层层地回到原问题，同时在此过程中得到对应层的解。

 
递归
递归地两个要素：   
> 1.基线条件（Base Case）：循环调用的结束。也就是上面说的可以直接求解的“最小子问题”。     
> 2.递归条件（Recursive Case）：继续调用自己的条件。也就是将问题继续分解。

例如：n! = n*(n-1)*(n-2)***1
 
```python 
def f(n):
    if n == 1:		# 基线条件
        return 1
    else:			# 递归条件
        return n * f(n)
```

**分治的应用例子:**

查找算法：二分法（Binary Search）      
排序算法：快速排序（Quick Sort）、归并排序（Merge Sort）      
最接近点对问题（ Closest Pair of Points）     
Strassen矩阵乘法（ Strassen’s Algorithm）    
傅里叶变换（ Cooley–Tukey Fast Fourier Transform (FFT) algorithm）     

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
