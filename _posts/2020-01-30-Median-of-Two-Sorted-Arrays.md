---
layout: post
title:  "Median of Two Sorted Arrays"
date:   2020-01-30 22:51:06 +0800
categories: algorithm
---

[问题描述]: https://leetcode.com/problems/median-of-two-sorted-arrays/

```plain
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty
```

思路是binary search，中位数的意义就是将集合两等分，左半边所有元素小于右半边所有元素，对于两个数组也是如此，只要我们可以满足两个条件：1.等分 2.左半边所有元素小于右半边所有元素。

形式化描述就是：

```c
1) len(left_part) == len(right_part)
2) max(left_part) <= min(right_part)
```

当然了，当总个数为奇数的时候，划分未必完全相等，不过也很好处理。最终希望达到的状态就是下面的情况：

```c
      left_part          |        right_part
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

为了满足条件1)，需要左右两边元素个数相等，或者说左边的元素加起来是整体的一半：

```c
i + j == m - i + n - j
or
i = 0~m, j = (m + n + 1) / 2 - i
```

这里让i在0~m中取值，j就是一半元素个数减去i，之所以要m + n + 1是为了在奇数的情况下，使得左半部分多出一个元素，当然，让右半部分多出一个元素来也可以。这里还要解释一下关于i的取值，i的取值并不是数组的下标，而是可以划分的位置，对于有m个元素的数组，共有m+1个可以划分的地方。

为了满足条件2)，需要

```c
A[i-1] <= B[j] && B[j-1] <= A[i]
```

然后就可以开始算法的核心部分了，以其中一个数组为主进行二分：有些特殊地方写在了代码的注释中：

```python
def findMedianSortedArrays(self, nums1, nums2):
    MAX = 9999999999
    MIN = -999999999
    # 一定是对小数组做二分 是为了(x + y + 1) // 2 - partitionX这里不出现负数
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    x, y = len(nums1), len(nums2)
    # partition并不代表元素下标 而是两元素之间的一个划分
    low, high = 0, x

    while low <= high:
        # 对小数组进行二分
        partitionX = (low + high) // 2
        # 如果是x+y是偶数 加1除以二没有影响 如果是奇数+1除以二就导致partitionX + partitionY是一半多一个元素
        partitionY = (x + y + 1) // 2 - partitionX

        # 分别计算两个数组左半边的最大值和右半边的最小值
        # 如果左半边为空 也就是没有元素在它左边 根据划分的含义 partition == 0 此时设置为MIN 表示比右边任何都小
        # 如果右半边为空 表示所有元素都在左边 也就是partition等于x或y 此时设置minRigh为MAX 表示比左边任何都大
        # 这里的边界条件是为了在下面的测试中 不出现特出情况
        maxLeftX = MIN if partitionX == 0 else nums1[partitionX - 1]
        minRightX = MAX if partitionX == x else nums1[partitionX]

        maxLeftY = MIN if partitionY == 0 else nums2[partitionY - 1]
        minRightY = MAX if partitionY == y else nums2[partitionY]

        # 注意这里是交叉的 x的左边最大 小于y的右边最小；y的左边最大小于x的右边最小
        if maxLeftX <= minRightY and maxLeftY <= minRightX:
            if (x + y) % 2 == 0:
                return (max(maxLeftX, maxLeftY) + min(minRightX, minRightY)) / 2.0
            return max(maxLeftX, maxLeftY)
        # 如果x的左边大了 那就是再从左半边找 设置high为partitionX - 1
        elif maxLeftX > minRightY:
            high = partitionX - 1
        # 如果x的左边小了 就像大的方向去找 设置low为partition + 1 即可
        else:
            low = partitionX + 1
```


