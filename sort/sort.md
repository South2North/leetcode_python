[toc]

# 1. 介绍
## 1.1 排序算法分类
1. 内部排序: 数据记录在内存中进行排序
2. 外部排序: 因排序的数据很大，一次不能容纳全部的排序记录，在排序过程中需要访问外存
<div align="center"><img src="./intro.webp" width="80%"></div>


## 1.2 关于时间复杂度
1. 平方阶($O(n^2)$)排序 各类简单排序：*直接插入、直接选择和冒泡排序*
2. 线性对数阶($O(n\log(n))$)排序：*快速排序、堆排序和归并排序*
3. $O(n^{1+§})$排序，§是介于0和1之间的常数：*希尔排序*
4. 线性阶($O(n)$)排序：*基数排序*，此外还有 *桶、箱排序*

## 1.3 关于稳定性
1. 稳定的排序算法：*冒泡排序、插入排序、归并排序和基数排序*
2. 不稳定的排序算法：*选择排序、快速排序、希尔排序、堆排序*

# 2. 细节
## 2.1 冒泡排序
- 步骤
1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。
<div align="center"><img src="./bubble.gif" width="80%"></div>

- 实现
```python
def bubble_sort(arr):
    '''
    如果没有逆序对，提前退出循环
    '''
    n = len(arr)
    for i in range(n-1):
        swapped = False
        for j in range(n-1-i):
            if arr[j] > arr[j+1]:
                arr[j],arr[j+1] = arr[j+1],arr[j]
                swapped = True
        # print(arr)
        if not swapped:
            break
    return arr
```

## 2.2 选择排序
- 步骤
1. 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。
2. 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
3. 重复第二步，直到所有元素均排序完毕。
<div align="center"><img src="./select.gif" width="80%"></div>

- 实现
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min = i
        for j in range(i+1, n):
            if arr[j] < arr[min]:
                min = j
        arr[i],arr[min] = arr[min],arr[i]
        # print(arr)
    return arr
```

## 2.3 插入排序
- 步骤
1. 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。
2. 从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。）
<div align="center"><img src="./insert.gif" width="80%"></div>

- 实现
```python
def insertion_sort(arr):
    n = len(arr)
    for i in range(n):
        cursor = arr[i]
        pos = i
        while pos > 0 and arr[pos-1] > cursor:
            # 大于cursor就一直往前移，前面的数往后补位
            arr[pos] = arr[pos-1]
            pos = pos-1
        # 最后再赋值
        arr[pos] = cursor
        # print(arr)
    return arr
```

## 2.4 希尔排序
希尔排序，也称递减增量排序算法，是**插入排序的一种更高效的改进版本**。但希尔排序是非稳定排序算法。
希尔排序是基于插入排序的以下两点性质而提出改进方法的：
1. 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率
2. 插入排序一般来说是低效的，因为**插入排序每次只能将数据移动一位**
**基本思想**：先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录“基本有序”时，再对全体记录进行依次直接插入排序。

- 步骤
1. 选择一个增量序列$t_1$, $t_2$, ... ,$t_k$，其中$t_i>t_j$, $t_k=1$；（序列是递减的）
2. 按增量序列个数 k，对序列进行 k 趟排序；
3. 每趟排序，根据对应的增量$t_i$，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。
<div align="center"><img src="./shell.gif" width="80%"></div>

- 实现
```python
def shell_sort(arr):
    '''
    相当于套了一个gap的插入排序：每次排序的对象是数组中间隔gap的元素
    '''
    n = len(arr)
    gap = n//2
    while gap > 0: # gap=0是循环出口
        for i in range(gap, n):
            cursor = arr[i]
            pos = i
            while pos >= gap and arr[pos-gap] > cursor:
                arr[pos] = arr[pos-gap]
                pos -= gap
            arr[pos] = cursor
        # print(arr)
        gap = gap//2
    return arr
```


## 2.5 归并排序
- 步骤
1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；
2. 设定两个指针，最初位置分别为两个已经排序序列的起始位置；
3. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；
4. 重复步骤 3 直到某一指针达到序列尾；
5. 将另一序列剩下的所有元素直接复制到合并序列尾。
<div align="center"><img src="./merge.gif" width="80%"></div>

- 实现
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr)//2
    left,right = arr[:mid],arr[mid:]
    return merge(merge_sort(left), merge_sort(right), arr.copy())

def merge(left, right, merged):
    l,r = 0,0
    while l < len(left) and r < len(right):
        if left[l] <= right[r]:
            merged[l+r] = left[l]
            l+=1
        else:
            merged[l+r] = right[r]
            r+=1
    for l in range(l, len(left)): # 注意这里的l和r
        merged[l+r] = left[l]
    for r in range(r, len(right)):
        merged[l+r] = right[r]
    return merged
```

## 2.6 快速排序
快速排序使用分治法（Divide and conquer）策略来把一个串行（list）分为两个子串行（sub-lists）。本质上来看，快速排序应该算是在**冒泡排序基础上的递归分治法**。
快速排序通常明显比其他 Ο(nlogn) 算法更快，因为它的**内部循环（inner loop）可以在大部分的架构上很有效率地被实现出来**。
- 步骤
1. 从数列中挑出一个元素，称为 “基准”（pivot）;
2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；
<div align="center"><img src="./quick.gif" width="80%"></div>

- 实现
```python
def partition(arr, low, high):
    i = low-1 # i是小于pivot的数的索引
    pivot = arr[high]
    
    for j in range(low, high):
        # 遍历low到high，将所有小于pivot的数都提到前面来
        if arr[j] < pivot:
            i += 1
            arr[i],arr[j]=arr[j],arr[i]

    # 将pivot提到索引i之后        
    arr[i+1],arr[high]=arr[high],arr[i+1]
    return (i+1)

def quick_sort(arr, low, high):
    if low < high:
        pivot = partition(arr, low, high)
        quick_sort(arr, low, pivot-1)
        quick_sort(arr, pivot+1, high)
    return arr

########################################

def qsort(arr):
    '''
    简化版，利用了python list的特点
    速度很慢，严格意义上不算快排
    '''
    if len(arr) <= 1:
        return arr
    pivot = arr[-1]
    return qsort([x for x in arr[:-1] if x < pivot]) + [pivot] + qsort([x for x in arr[:-1] if x >= pivot])
```

## 2.7 堆排序

## 2.8 计数排序

## 2.9 桶排序

## 2.10 基数排序
