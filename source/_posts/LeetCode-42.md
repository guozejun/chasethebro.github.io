---
title: LeetCode 42
toc: true
date: 2020-11-11 17:43:58
tags: [LeetCode]
categories:
    - LeetCode
---



## 思路

经典的双指针问题，首先让左右两指针分别指向数组的头和尾，并维护两个变量，`left_max`和`right_max`用来存储当前左侧和右侧的最大值。假设，当前左侧的柱形高度低于右侧，那么如果左侧的柱形低于`left_max`的高度，那么当前柱形的上方一定能积水，否则，一定没有积水，那么我们就将`left_max`的高度更新为当前柱形的高度。右侧同理。那么用此方法，使用双指针遍历完所有的柱形之后，总的积水量就可以得出来了。

## 代码

```cpp
class Solution {
public:
int trap(vector<int>& height)
{
    int left = 0, right = height.size() - 1;
    int ans = 0;
    int left_max = 0, right_max = 0;
    while (left < right) {
        if (height[left] < height[right]) {
            height[left] >= left_max ? 
            (left_max = height[left]) : ans += (left_max - height[left]);
            ++left;
        }
        else {
            height[right] >= right_max ? 
            (right_max = height[right]) : ans += (right_max - height[right]);
            --right;
        }
    }
    return ans;
}
};
```



## 参考资料
> - [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

