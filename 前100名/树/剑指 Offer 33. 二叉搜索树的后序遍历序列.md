## 题目
输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。
## 思路
用右子树的节点不断和根节点比较，递归所有的
```java
class Solution {
    int[] postorder;
    public boolean verifyPostorder(int[] postorder) {
        this.postorder=postorder;
        return recure(0,postorder.length-1);
    }
    boolean recure(int start,int root){
        if(start>=root) return true;
        int rightroot=start;
        while(postorder[rightroot]<postorder[root]) rightroot++;
        int leftroot=rightroot-1;
        while(postorder[rightroot]>postorder[root]) rightroot++;
        return root==rightroot&&recure(start,leftroot)&&recure(leftroot+1,root-1);
    }
}
```
