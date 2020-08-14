# 剑指 Offer 11. 旋转数组的最小数字（简单）

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例 1：

输入：[3,4,5,1,2]
输出：1

示例 2：

输入：[2,2,2,0,1]
输出：0

### 我认为的二分查找 error
```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        if(numbers.size() == 1){return numbers[0];}
        int left = 1, right = numbers.size()-1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(numbers[mid] < numbers[mid - 1]){
                return numbers[mid];
            }
            if(numbers[mid] < numbers[0]){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return numbers[0];
    }
};
```

### 正确的二分查找
[3, 4, 5, 6, 1, 2, 3]
左：[3, 4, 5, 6], 右：[1, 2, 3]

if numbers[mid] > numbers[right]  ---> in left array  ---> left = mid + 1
if numbers[mid] < numbers[right] ---> in right array ---> right = mid
if numbers[mid] == numbers[right] ---> don't know ---> right -- 缩小范围

### 二分查找：  
[题解链接](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/solution/xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-de-zui-1-8/)
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(nums[mid] < nums[right]){
                right = mid;
            }else if(nums[mid] > nums[right]){
                left = mid + 1;
            }else{
                right --;   //精髓
            }
        }
        return nums[left];
    }
};
```
