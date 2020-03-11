[面试题57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

示例 1：
```sh
输入：nums = [2,7,11,15], target = 9
输出：[2,7] 或者 [7,2]
```

示例 2：
```sh
输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]
```

限制：
```sh
1 <= nums.length <= 10^5
1 <= nums[i] <= 10^6
```

代码：
```python
'''
思路：双指针
    i,j分别指向头尾，
    当i<j
    若两数之和==target返回，若两数之和>target，则j-=1，若<target，则i+=1
'''
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        if len(nums)<2:
            return None
        i,j=0,len(nums)-1
        while i<j:
            if nums[i]+nums[j]==target:
                return [nums[i],nums[j]]
            elif nums[i]+nums[j]<target:
                i+=1
            else:
                j-=1
        return None
```
