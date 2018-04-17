# Search

* 查找的定义：
> 给定元素a，在数组、列表、树、图等数据结构中查找给定元素a是否存在，如果存在返回元素所在的索引，否则返回-1

* 二分查找
```python
"""
二分查找的递归实现
"""
def binary_search(arr,start,end,target):
    if start<=end:
        mid = (start+end)//2
        if arr[mid] == target:
            return mid
        if arr[mid]>target:
            return binary_search(arr,start,mid-1,target)
        else:
            return binary_search(arr,mid+1,end,target)
    return -1
```

* 二分法的扩展：
    1. 旋转递增序列后target目标查找
    2. 递增序列元素重复出现时查找target起始索引

* 查找重复出现元素组成的递增序列中目标元素索引的起始位置
> 二分查找变形，leetcode34[Search for a Range]
```python
"""
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
"""
def searchRange(nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if nums:
            left,right = 0,len(nums)-1
            index = -1
            while left<=right:
                mid = (left+right)//2
                if target == nums[mid]:
                    index = mid
                    break
                elif target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            if index != -1:
                start,end = index,index
                while start-1 >= 0 and nums[start-1] == target:
                    start -= 1
                while end+1 < len(nums) and nums[end+1] == target:
                    end += 1
                return [start,end]
        return [-1,-1]
```
