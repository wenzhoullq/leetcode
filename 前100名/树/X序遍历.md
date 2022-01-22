* 144.二叉树的前序遍历
### 题目
给你二叉树的根节点 root ，返回它节点值的 前序 遍历。
解法一：递归
```java
class Solution {
    List<Integer> ans=new ArrayList<>();
    void dfs(TreeNode root){
        if(root==null) return ;
        ans.add(root.val);
        dfs(root.left);
        dfs(root.right);
    }
    public List<Integer> preorderTraversal(TreeNode root) {
        dfs(root);
        return ans;
    }
}
```
解法二：栈
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        Stack<TreeNode> s=new Stack<>();
        List<Integer> ans=new LinkedList<>();
        while(root!=null||!s.isEmpty()){
            while(root!=null){
                ans.add(root.val);
                s.push(root);
                root=root.left;
            }
            root=s.pop();
            root=root.right;
        }
        return ans;
    }
}
```
解答三：Morris
Morris可以将空间复杂度降为O(1),但是会将树变为链表
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        TreeNode pre=root;
        List<Integer> ans=new LinkedList<>();
        while(root!=null){
            pre=root.left;
            if(pre!=null){   
                while(pre.right!=null&&pre.right!=root) pre=pre.right;
                if(pre.right==null){
                    pre.right=root;
                    ans.add(root.val);
                    root=root.left;
                    continue;
                } 
                else pre.right=null;
            }
            else    ans.add(root.val);
                root=root.right;
        }
        return ans;
    }
}
```
* 94.二叉树的中序遍历
### 题目
给你二叉树的根节点 root ，返回它节点值的 中序 遍历。
解法一：递归
```java
class Solution {
    List<Integer> ans=new ArrayList<Integer>();
    void dfs(TreeNode root){
        if(root==null) return ;
        dfs(root.left);
        ans.add(root.val);
        dfs(root.right);
    }
    public List<Integer> inorderTraversal(TreeNode root) {
        dfs(root);
        return ans;
    }
}
```
解法二：栈
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans=new LinkedList<>();
        Stack<TreeNode> s=new Stack<>();
        while(root!=null||!s.isEmpty()){
            while(root!=null){
                s.push(root);
                root=root.left;
            }
            root=s.pop();
            ans.add(root.val);
            root=root.right;
        }
        return ans;
    }
}
```
解答三：Morris
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        TreeNode pre=root;
        List<Integer> ans=new LinkedList<>();
        while(root!=null){
            if(root.left!=null){
                pre=root.left;
                while(pre.right!=null) pre=pre.right;
                pre.right=root;
                TreeNode temp=root;
                root=root.left;
                temp.left=null;
            }
            else {
                ans.add(root.val);
                root=root.right;
            }
        }
        return ans;
    }
}
```
* 145.二叉树的后序遍历
### 题目
给你二叉树的根节点root，返回它节点值的后序遍历。
解法一：递归
```java
class Solution {
    List<Integer> ans=new ArrayList<>();
    void dfs(TreeNode root){
        if(root==null) return ;
        dfs(root.left);
        dfs(root.right);
        ans.add(root.val);
    }
    public List<Integer> postorderTraversal(TreeNode root) {
        dfs(root);
        return ans;
    }
}
```
解法二：栈
前序遍历是：根，左，右;后序遍历是左，右，根，那么前序遍历反方向操作，再使用头插法就可以得到后续遍历
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        Stack<TreeNode> s=new Stack<>();
        LinkedList<Integer> ans=new LinkedList<>();
        while(root!=null||!s.isEmpty()){
            while(root!=null){
                s.push(root);
                ans.addFirst(root.val);
                root=root.right;
            }
            root=s.pop(); 
            root=root.left;
        }
        return ans;
    }
}

```
解答三：Morris
前序的Morris的反方向操作，再使用头插法
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        TreeNode pre;
        LinkedList<Integer> ans=new LinkedList<>();
        while(root!=null){
            pre=root.right;
            if(pre!=null){
                while(pre.left!=null&&pre.left!=root) pre=pre.left;
                if(pre.left==null){
                    pre.left=root;
                    ans.addFirst(root.val);
                    root=root.right;
                    continue;
                }
                else{
                    pre.right=null;
                }
                
            }
            else ans.addFirst(root.val); 
            root=root.left;
        }
        return ans;
    }
}
```
