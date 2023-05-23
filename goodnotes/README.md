# 代码碎片

## 交换

```cpp
int tmp = s[i];
s[i] = s[j];
s[j] = tmp;

s[i] ^= s[j];
s[j] ^= s[i];
s[i] ^= s[j];
```

## 位操作

```cpp
left + ((right - left) >> 1)
left + ((right - left) / 2)
(left + right) / 2
```

## 二分查找

1. sorted
2. init left, right, middle -> judge nums[middle] ~ target -> set left/right to middle

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1; // 定义target在左闭右闭的区间里，[left, right]

        while (left <= right) { // 当left==right，区间[left, right]依然有效，所以用 <=
            int middle = left + ((right - left) / 2); // 防止溢出，等同于 (left + right)/2
            if (nums[middle] > target) {
                right = middle - 1; // target 在左区间，所以[left, middle - 1]
            } else if (nums[middle] < target) {
                left = middle + 1; // target 在右区间，所以[middle + 1, right]
            } else { // nums[middle] == target
                return middle; // 数组中找到目标值，直接返回下标
            }
        }
        // 未找到目标值
        return -1;
    }
};
```

1. `[left, right)` methods

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size(); // 定义target在左闭右开的区间里，即：[left, right)
        while (left < right) { // 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
            int middle = left + ((right - left) >> 1);
            if (nums[middle] > target) {
                right = middle; // target 在左区间，在[left, middle)中
            } else if (nums[middle] < target) {
                left = middle + 1; // target 在右区间，在[middle + 1, right)中
            } else { // nums[middle] == target
                return middle; // 数组中找到目标值，直接返回下标
            }
        }
        // 未找到目标值
        return -1;
    }
};
```

## 暴力解法

### 多层循环

1. takses `O(n^m)` m is the num of layers

```cpp
// 时间复杂度：O(n^2)
// 空间复杂度：O(1)
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for (int i = 0; i < size; i++) {  // 外层循环
            if (nums[i] == val) { // 发现需要移除的元素，就将数组集体向前移动一位
                for (int j = i + 1; j < size; j++) {  // 内层循环
                    nums[j - 1] = nums[j];
                }
                i--; // 因为下标i以后的数值都向前移动了一位，所以i也向前移动一位
                size--; // 此时数组的大小-1
            }
        }
        return size;

    }
};
```

## 双指针

```cpp
// 双指针
void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1; i < s.size()/2; i++, j--) {
            swap(s[i],s[j]);
        }
```

### 快慢指针

1. 慢指针用于记录
2. 快指针一直前进
3. 慢指针根据条件前进
4. 赋值后慢指针前进，赋值操作并不越位（不会影响快指针未遍历的位置）

```cpp
// 时间复杂度：O(n)
// 空间复杂度：O(1)
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0;  // 慢指针用于记录
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++) {  // 快指针一直前进
            if (val != nums[fastIndex]) {  // 慢指针根据条件前进
                nums[slowIndex++] = nums[fastIndex];  // 赋值后慢指针前进
            }
        }
        return slowIndex;
    }
};
```

### 双向指针

1. 相向双指针

```cpp
// 相向双指针，确保移动最少的数字
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int left = 0;
        int right = nums.size() - 1;  // 闭区间写法

        while (left <= right) {  // 闭区间写法
            // 找左边等于val的元素
            while (left <= right && nums[left] != val){
                ++left;
            }
            // 找右边不等于val的元素
            while (left <= right && nums[right] == val) {
                --right;
            }
            // 将右边不等于val的元素覆盖左边等于val的元素
            if (left < right) {
                nums[left++] = nums[right--];
            }
        }
        return left;   // left一定指向了最终数组末尾的下一个元素
    }
};
```

## 排序