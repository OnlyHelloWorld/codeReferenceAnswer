出现在:腾讯

数组中第K大值  练习地址:https://leetcode-cn.com/problems/kth-largest-element-in-an-array/

1,利用快排思想求解
```
class Solution {
    public int findKthLargest(int[] nums, int k) {
        //根据快排思想
        int len = nums.length;
        quickSort(nums,0,len-1,len-k);
        return nums[len-k];
    }
    
    private void quickSort(int[] nums,int l,int r,int k){
        int left = l;   int right = r;
        int mid = nums[left];
        while(l < r){
            //快排常规操作,找到大于小于基数的下标
            while(l < r && mid <= nums[r] )
                r--;
            while(l < r && mid >= nums[l] )
                l++;
            //交换int[l]和int[r]值
            if(l < r){
                int t = nums[l];
                nums[l] = nums[r];
                nums[r] = t;
            }
        }
        //把基数值与此时l位置值交换
        nums[left] = nums[l];
        nums[l] = mid;
        //此时如果l在第K个位置,直接返回即可,否则递归下去
        if(l == k){
            return;
        }else if(l < k){
            quickSort(nums,l+1,right,k);
        }else if(l > k){
            quickSort(nums,left,l-1,k);
        }
    }
}
```

2,利用堆求解(利用java优先队列)
先来复习一下优先队列基本用法(其实我们还可以重写比较器来实现大顶堆,PriorityQueue默认是小顶堆)
```
import java.util.PriorityQueue;
import java.util.Queue;
import java.util.Random;
 
public class PriorityQueueHeapSort {
    public static void main(String[] args) {
        //优先队列自然排序示例
        Queue<Integer> integerPriorityQueue = new PriorityQueue<>(7);
        Random rand = new Random();
        for(int i=0;i<7;i++){
            integerPriorityQueue.add(new Integer(rand.nextInt(100)));
        }
        for(int i=0;i<7;i++){
            Integer in = integerPriorityQueue.poll();
            System.out.println("Processing Integer:"+in);
        }
    }
}
```
------------------------------------------------------------
那这样就简单很多了,我们只需要把数组放进大小为len-K的优先队列里,然后取出最大值即可
```
class Solution {
    //优先队列不能初始化大小为0,所以对k=nums.length的情况特殊处理
    public int findKthLargest(int[] nums, int k) {
        if(nums.length == k){
            int min = nums[0];
            for(int i = 0;i < nums.length;i++){
                min = min < nums[i] ? min : nums[i];
            }
            return min;
        }
        //根据优先队列
        int len = nums.length;
        Queue<Integer> pQueue = new PriorityQueue<>(len-k);
        for(int i = 0;i < len;i++){
            pQueue.add(new Integer(nums[i]));
        }
        for(int i = 0;i < len-k;i++)
            pQueue.poll();
        return pQueue.poll();
    }
}
```
