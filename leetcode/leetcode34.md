出现在:字节跳动

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
你的算法时间复杂度必须是 O(log n) 级别。
练习地址:https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/

看到已经排序的数组,并且对时间复杂度有要求,我们应该想到利用二分的思想来求解

代码实现:
```
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res = new int[]{-1,-1};
        int left = 0;int right = nums.length-1;
        while(left <= right){
            int mid = (left + right) >> 1;
            if(nums[mid] == target){
                while(mid >= left && nums[mid] == target)
                    mid--;
                res[0] = mid+1;
                mid = (left + right) >> 1;
                while(mid <= right && nums[mid] == target)
                    mid++;
                res[1] = mid-1;
                break;
            }else if(nums[mid] > target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return res;
    }
}
```
