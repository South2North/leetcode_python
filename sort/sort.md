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
def bubble_sort1(arr):
    '''
    循环次数固定
    '''
    n = len(arr)
    for i in range(n-1): # 注意这里是n-1，否则会越界
        for j in range(n-1-i):
            if arr[j] > arr[j+1]:
                arr[j],arr[j+1] = arr[j+1],arr[j]
        # print(arr)
    return arr

def bubble_sort2(arr):
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

## 2.5 归并排序

## 2.6 快速排序

## 2.7 堆排序

## 2.8 计数排序

## 2.9 桶排序

## 2.10 基数排序
