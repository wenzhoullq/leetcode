* 752.打开转盘锁.md
## 题目
你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为 '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，你需要给出解锁需要的最小旋转次数，如果无论如何不能解锁，返回 -1 。
## 思路
因为是取**最短路径长**，因此是宽度优先
* 单向BFS
```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Queue<String> q=new LinkedList<>();
        HashSet<String> s=new HashSet<>();
        for(String num:deadends){
            if(num=="0000") return -1;
            s.add(num); 
        }
        q.add("0000");
        int step=0;
        while(!q.isEmpty()){
            int length=q.size();
            for(int i=0;i<length;i++){
                String temp= q.poll();
                if(temp.equals(target)) return step;
                if(s.contains(temp)) continue;
                s.add(temp);
                for(int j=0;j<4;j++){
                    String prestr=pre(temp,j),nextstr=next(temp,j);
                    if(!s.contains(prestr))q.add(prestr);
                    if(!s.contains(nextstr))q.add(nextstr);
                }
            }
            step++;
        }
        return -1;
    }
    String pre(String str,int i){
        char[] s=str.toCharArray();
        if(s[i]-1<'0') s[i]='9';
        else s[i]=(char)(s[i]-1);
        return new String(s);
    }
     String next(String str,int i){
        char[] s=str.toCharArray();
        if(s[i]+1>'9') s[i]='0';
        else s[i]=(char)(s[i]+1);
        return new String(s);
    }
}
```
* 双向BFS
另一个从target出发，如果碰到对方出现过的，那么相加步数即可
```java
class Solution {
    public int openLock(String[] deadends, String target) {
        HashSet<String> s=new HashSet<>();
        HashMap<String,Integer> m1=new HashMap<>();
        HashMap<String,Integer> m2=new HashMap<>();
        if(target.equals("0000")) return 0;
        for(int i=0;i<deadends.length;i++){
            if(deadends[i].equals("0000")) return -1;
            s.add(deadends[i]);
        }
        Queue<String> q1=new LinkedList<>();
        Queue<String> q2=new LinkedList<>();
        q1.add("0000");
        q2.add(target);
        int step1=0,step2=0;
        while(!q1.isEmpty()&&!q2.isEmpty()){
            int size1=q1.size();
            for(int i=0;i<size1;i++){
                String temp=q1.poll();
                if(s.contains(temp)||m1.containsKey(temp)) continue;
                if(m2.containsKey(temp)) return step1+m2.get(temp);
                m1.put(temp,step1);
                for(int j=0;j<4;j++){
                    String pre=prestr(temp,j),next=nextstr(temp,j);
                    q1.add(pre);
                    q1.add(next);
                    
                }
            }
            step1++;
            int size2=q2.size();
            for(int i=0;i<size2;i++){
                String temp=q2.poll();
                if(s.contains(temp)||m2.containsKey(temp)) continue;
                if(m1.containsKey(temp)) return m1.get(temp)+step2;
                m2.put(temp,step2);
                for(int j=0;j<4;j++){
                    String pre=prestr(temp,j),next=nextstr(temp,j);
                    q2.add(pre);
                    q2.add(next);
                }
            }
            step2++;
        }
        return -1;
    }
    String prestr(String str,int i){
        int num=str.charAt(i)-'0';
        char[] temp=str.toCharArray();
        temp[i]=(char)(((num-1)%10+10)%10+'0');
        return new String(temp);
    }
    String nextstr(String str,int i){
        int num=str.charAt(i)-'0';
        char[] temp=str.toCharArray();
        temp[i]=(char)((num+1)%10+'0');
        return new String(temp);
    }
}
```
* 542. 01 矩阵

## 题目
给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

 

示例 1：



输入：mat = [[0,0,0],[0,1,0],[0,0,0]]
输出：[[0,0,0],[0,1,0],[0,0,0]]

## 思路
关键词**最短**，因此BFS，把为0的点加入队列，然后去挨个访问
```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        // 首先将所有的 0 都入队，并且将 1 的位置设置成 -1，表示该位置是 未被访问过的 1
        Queue<int[]> queue = new LinkedList<>();
        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    queue.offer(new int[] {i, j});
                } else {
                    matrix[i][j] = -1;
                } 
            }
        }
        
        int[] dx = new int[] {-1, 1, 0, 0};
        int[] dy = new int[] {0, 0, -1, 1};
        while (!queue.isEmpty()) {
            int[] point = queue.poll();
            int x = point[0], y = point[1];
            for (int i = 0; i < 4; i++) {
                int newX = x + dx[i];
                int newY = y + dy[i];
                // 如果四邻域的点是 -1，表示这个点是未被访问过的 1
                // 所以这个点到 0 的距离就可以更新成 matrix[x][y] + 1。
                if (newX >= 0 && newX < m && newY >= 0 && newY < n 
                        && matrix[newX][newY] == -1) {
                    matrix[newX][newY] = matrix[x][y] + 1;
                    queue.offer(new int[] {newX, newY});
                }
            }
        }

        return matrix;
    }
}

```
