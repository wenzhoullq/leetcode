* 334.递增的三元子序列

## 题目
给你一个整数数组 nums ，判断这个数组中是否存在长度为 3 的递增子序列。

如果存在这样的三元组下标 (i, j, k) 且满足 i < j < k ，使得 nums[i] < nums[j] < nums[k] ，返回 true ；否则，返回 false 。
## 思路
最长上升子序列
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int length=0;
        int[] tail=new int[nums.length];
        for(int i=0;i<nums.length;i++){
            int left=0,right=length;
            while(left<right){
                int mid=(left+right)/2;
                if(tail[mid]<nums[i]) left=mid+1;
                else right=mid;
            }
            tail[left]=nums[i];
            if(right==length) {
                length++;
                if(length>=3) return true;
            }
        }
        return false;
    }
}
```
