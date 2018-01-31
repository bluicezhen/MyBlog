---
title: LeetCode - Median of Two Sorted Arrays
date: 2018-01-31 10:25:11
categories: 算法拾遗
tags: 
    - 算法
    - LeetCode
    - 排序
    - C
summary: LeetCode题目《Median of Two Sorted Arrays》的 $O(log((m+n)/2))$ 解法。
---

**问题描述：**There are two sorted arrays nums1 and nums2 of size m and n respectively. Find the median of the two sorted arrays. The overall run time complexity should be $O(log(m+n))$.

**Example：**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

****

这道题最直观的解法是（伪代码 描述）:

```python
def findMedianSortedArrays(nums1, nums2):
    nums3 = nums1 + nums2
    sorted(nums3)
    if nums3.length is even:
        return (nums3[nums3.length / 2] + nums3[nums3.length / 2 - 1]) / 2
    return nums3[(nums3.length - 1) / 2]
```

即合并排序取中间值。

从伪代码易得，该算法的时间复杂度即排序算法的时间复杂度。没有仔细分析，但应该可以做到平均$O(log(m+n))$。

但合并已排序数组可以通过依次比较nums1和nums2来做，利用已经做好的排序。算法不是很好描述，比较好举例，已题目数据为例

```python
nums1[0] < nums2[0] so nums3[0] = nums1[0]
nums1[1] > nums2[0] so nums3[1] = nums2[0]
nums3[0] = nums1[1]
```

此外还有两个可以优化的点：

- 得到的数据不需要完整，需要计算量数组长度和的一半
- 我们不需要目标数组，所以不需要创建新数组

综上，优化后时间复杂度为：$O(\frac{m+n}{2})$，空间复杂度为：$O(1)$。完整代码（C语言描述）：

```c
double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size) {
    int nums3Size = nums1Size + nums2Size;

    bool isNum3SizeEven = (nums3Size % 2) == 0;
    int l1 = 0;
    int l2 = 0;
    int l3 = isNum3SizeEven ? (nums3Size / 2) : ((nums3Size - 1) / 2);
    int *p1 = nums1;
    int *p2 = nums1;

    for (int i = 0; i <= l3; i++) {
        if (*nums1 < *nums2) {

            if (l1 == nums1Size) {
                if (i == (l3 - 1)) {
                    p1 = nums2;
                } else if (i == l3) {
                    p2 = nums2;
                }
                nums2++;
                l2++;
            } else {
                if (i == (l3 - 1)) {
                    p1 = nums1;
                } else if (i == l3) {
                    p2 = nums1;
                }
                nums1++;
                l1++;
            }
        } else {
            if (l2 == nums2Size) {
                if (i == (l3 - 1)) {
                    p1 = nums1;
                } else if (i == l3) {
                    p2 = nums1;
                }
                nums1++;
                l1++;
            } else {
                if (i == (l3 - 1)) {
                    p1 = nums2;
                } else if (i == l3) {
                    p2 = nums2;
                }
                nums2++;
                l2++;
            }
        }
    }

    return isNum3SizeEven ? (((double)*p1 + (double)*p2) / 2) : *p2;
}
```



