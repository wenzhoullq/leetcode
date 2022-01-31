* 072.编辑距离
## 题目
给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符
## 思路
动态规划，两重for循环，但是需要注意的是，dp[i][j]的意思是`str1中长度为i的子串（从字符串开始）变成str2中长度为j的子串的需要变化的次数`，因此需要先开始初始化i为0的str1添加动作和j为0的str1的删除动作
，str1 = "horse", str2 = "ros"中，当i和j均为1时候，如果是添加动作，就是_ (_ 表示为空)变成 r;如果是删除动作，就是rh删除h变为r(这个r在初始化的时候添加了);如果是代替动作,就是_h和_r,h被r替代
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] dp=new int[word1.length()+1][word2.length()+1];
        for(int i=1;i<=word1.length();i++) dp[i][0]=dp[i-1][0]+1;
        for(int i=1;i<=word2.length();i++) dp[0][i]=dp[0][i-1]+1;
        for(int i=1;i<=word1.length();i++){
            for(int j=1;j<=word2.length();j++){
                if(word1.charAt(i-1)==word2.charAt(j-1)) dp[i][j]=dp[i-1][j-1];
                else dp[i][j]=Math.min(dp[i-1][j],Math.min(dp[i][j-1],dp[i-1][j-1]))+1;
            }
        }
        return dp[word1.length()][word2.length()];
    }
}
```
* 1143. 最长公共子序列
## 题目
给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。
## 思路
经典二维dp题，碰到不同的抄前面，碰到需要改的根据实际情况进行计算
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int length1=text1.length(),length2=text2.length();
        int[][] dp=new int[length1+1][length2+1];
        for(int i=1;i<=length1;i++){
            char temp=text1.charAt(i-1);
            for(int j=1;j<=length2;j++){
                if(text2.charAt(j-1)==temp) dp[i][j]=dp[i-1][j-1]+1;
                else dp[i][j]=Math.max(dp[i][j-1],dp[i-1][j]);
            }
        }
        return dp[length1][length2];
    }
}

```
* 718.最长重复子数组
## 题目
给两个整数数组 A 和 B ，返回两个数组中公共的、长度最长的子数组的长度。
## 思路
二维dp
```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int length1=nums1.length,length2=nums2.length,ans=0;
        int[][] dp=new int[length1+1][length2+1];
        for(int i=1;i<=length1;i++){
            int temp=nums1[i-1];
            for(int j=1;j<=length2;j++){
                if(temp==nums2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                    ans=Math.max(ans,dp[i][j]);
                }  
            }
        }
        return ans;
    }
}

```
