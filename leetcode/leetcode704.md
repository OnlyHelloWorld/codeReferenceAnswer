出现在:腾讯

非递归二分查找   练习地址:https://leetcode-cn.com/problems/binary-search/submissions/
注:练习地址中没有要求求解方法,面试中禁止使用递归

简单二分操作,利用l<=r循环即可
```
class Solution {
    public int search(int[] nums, int target) {
        int r = nums.length-1;
        int l = 0;
        //二分,判断在左半部分还是在右半部分
        while(l <= r){
            int mid = (l+r)/2;
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] > target){
                r = mid - 1;
            }else if(nums[mid] < target){
                l = mid + 1;
            }
        }
        return -1;
    }
}
```

递归方法还是有必要学会(没有使用用例测试)
```
class Solution {
    public int search(int[] nums, int target,int low,int high) {
        if(low <= high){
            int mid = (low+high)/2;
            if(nums[mid] > target){
                return search(nums,target,low,mid-1);
            }else if(nums[mid] < target){
                return search(nums,target,mid+1,high);
            }else if(nums[mid] == target){
                return mid;
            }
        }
        return -1;
    }
}
```
