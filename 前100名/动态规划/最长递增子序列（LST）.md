# LST(最长递增子序列)
## 300.最长递增子序列
给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。
子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。
## 思路
最开始想到的就是`动态规划`，动态规划的时间复杂度是O(n²)
* 动态规划
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp=new int[nums.length];
        int ans=1;
        Arrays.fill(dp,1);
        for(int i=0;i<nums.length;i++){
            for(int j=0;j<i;j++){
                if(nums[i]>nums[j]) dp[i]=Math.max(dp[j]+1,dp[i]);
            }
            ans=Math.max(dp[i],ans);
        }
    return ans;
    }
}
```
* 二分 
二分将第二重循环降低为O(log n),最后的时间复杂度为O(nlogn)
主要是tail表，tail表记录的是当前长度的最小值，在二分的查找中，left所位于的都是**当前长度最小的数字**,如果right没有发生变动，则说明nums[i]一直是大于tail中的所有值的，因此长度+1
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tail= new int[nums.length];
        int length=0;
        for(int i=0;i<nums.length;i++){
            int left=0,right=length;
            while(left<right){
                int mid=(left+right)/2;
                if(nums[i]>tail[mid]) left=mid+1;
                else right=mid;
            }
            tail[left]=nums[i];
            if(right==length) length++;
        }
        return length;
    }
}
```
* 要求输出路径
在一个的数组上记为当前长度，然后符合长度的逆序输出
```java
class Solution {
    public int[] lengthOfLISPath(int[] nums) {
        int[] tail= new int[nums.length],index=new int[nums.length];
        int length=0;
        List<Integer> temp=new LinkedList();
        for(int i=0;i<nums.length;i++){
            int left=0,right=length;
            while(left<right){
                int mid=(left+right)/2;
                if(nums[i]>tail[mid]) left=mid+1;
                else right=mid;
            }
            tail[left]=nums[i];
            index[i]=left;
            if(right==length) length++;
        }
        for(int i=nums.length;i>=1;i--){
            if(index[i]==length){
                ans.addFirst(index[i]);
                length--;
            }
        }
        return ans;
    }
}
```
## 637.最长递增子序列个数
给定一个未排序的整数数组，找到最长递增子序列的**个数**。
## 思路
采用O(n²)的动态规划来，再新增一个count[]数组来，然后不断更新ans和count[]数组
```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int[] dp=new int[nums.length],count=new int[nums.length];
        int ans=1,maxlen=0;
        Arrays.fill(dp,1);Arrays.fill(count,1);
        for(int i=0;i<nums.length;i++){
            for(int j=0;j<i;j++){
                if(nums[i]>nums[j]){
                    if(dp[i]<dp[j]+1){
                        dp[i]=dp[j]+1;
                        count[i]=count[j];
                    }
                    else if(dp[i]==dp[j]+1){
                        count[i]+=count[j];
                    }
                }
            }
            if(dp[i]>maxlen){
                maxlen=dp[i];
                ans=count[i];
            }
            else if(dp[i]==maxlen){
                ans+=count[i];
            }
        }
        return ans;
    }
}
```
## 354. 俄罗斯套娃信封问题
给你一个二维整数数组 envelopes ，其中 envelopes[i] = [wi, hi] ，表示第 i 个信封的宽度和高度。

当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算 最多能有多少个 信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

注意：不允许旋转信封。
### 思路
二维的LST问题，主要是排序问题，对长度进行升序，但是对**长度相同宽度不同**的进行降序，这样就可以避免长度相同的信封的重复
```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        Arrays.sort(envelopes,(x,y)->x[0]!=y[0]?x[0]-y[0]:y[1]-x[1]);
        int length=0;
        int[] tail=new int[envelopes.length];
        for(int i=0;i<envelopes.length;i++){
            // while(i+1<envelopes.length&&envelopes[i][0]==envelopes[i+1][0]) continue;
            int left=0,right=length;
            while(left<right){
                int mid=(left+right)/2;
                if(envelopes[i][1]>tail[mid]) left=mid+1;
                else right=mid;
            }
            tail[left]=envelopes[i][1];
            if(right==length) length++;
            
        }
        return length;
    }
}
```
