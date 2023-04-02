# 704 二分查找

这题有两个解法，一是左闭右闭区间(即$[left,right]$)，二是左闭右开区间(即$[left, right)$)。

两种解法的要点是：

-   right的初始化位置
-   while的判断条件
-   当`nums[mid] > target`时，right的变化

## 左闭右闭区间

-   `right`需要初始化为`nums.size() - 1`。
-   `left == right`在区间$[left,right]$中是有意义的，所以`while`需要判断`left <= right`的情况。
-   当`nums[mid] > target`时，已经判断了`nums[mid] != target`，所以不需要将`nums[mid]`归入下次循环，即将`right`更新为`mid - 1`即可。

### 代码

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        //将target定义在左闭右闭区间内
        int right = nums.size() - 1;
        
        //当left==right时，[left, right]区间依然有效，所以使用<=
        while (left <= right)
        {   
            int mid = left + (right - left) / 2;	//防止溢出，等同于(left + right) / 2
            //也可以写为int mid = left + ((right - left) >> 1);
            if (nums[mid] < target)
                left = mid + 1;		//target更新为[mid + 1, right]
            else if (nums[mid] > target)
                right = mid - 1;	//target更新为[left, mid - 1]
            else
                return mid;
        }

        return -1;
    }
};
```

## 左闭右开区间

-   `right`需要初始化为`nums.size()`。
-   `left == right`在区间$[left,right)$中没有意义，所以`while`只需要判断`left < right`的情况。
-   由于是左闭右开区间，所以将`right`更新为`mid`时，`nums[right]`不会在循环比较范围内。

### 代码

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        //将target定义在左闭右闭区间内
        int right = nums.size();
        
        //当left==right时，[left, right]区间依然有效，所以使用<=
        while (left < right)
        {   
            int mid = left + (right - left) / 2;	//防止溢出，等同于(left + right) / 2
            //也可以写为int mid = left + ((right - left) >> 1);
            if (nums[mid] < target)
                left = mid + 1;		//target更新为[mid + 1, right)
            else if (nums[mid] > target)
                right = mid;	//target更新为[left, mid]
            else
                return mid;
        }

        return -1;
    }
};
```

