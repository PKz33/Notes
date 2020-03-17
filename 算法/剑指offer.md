## 剑指offer
- **二维数组中的查找**  
1. 题目描述：二维数组每一行从左到右递增，每一列从上到下递增，判断数组中是否存在给定整数  
2. 代码实现：  
```
  public boolean Find(int target, int [][] array) {
    int rows = array.length;
    int cols= array[0].length;
    int r = 0, c = cols - 1;
    while(r < rows && c >= 0 ) {
      if(array[r][c] == target) {
        return true;
      }else if(array[r][c] < target) {
        r++;
      }else {
        c--;
      }
    }
    return false;
  }
```  
- **从上往下打印二叉树**  
1. 题目描述：层次遍历二叉树  
2. 代码实现：  
```
  public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
    ArrayList<Integer> res = new ArrayList<>();
    if(root == null) {
      return res;
    }
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    while(!queue.isEmpty()) {
      int cnt = queue.size();
      while(cnt-- > 0) {
        TreeNode cur = queue.poll();
        if(cur == null) {
          continue;
        }
        res.add(cur.val);
        queue.add(cur.left);
        queue.add(cur.right);
      }
    }
    return res;
  }
```
