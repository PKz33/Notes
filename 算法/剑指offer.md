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
- **二进制中1的个数**  
1. 解题思路：`n & (n-1)`，将最右边的一个1变为0，其左边保持不变  
2. 代码实现：  
```
  public int NumberOf1(int n) {
    int res = 0;
    while(n!=0) {
      res++;
      n &= (n-1);
    }
    return res;
  }
```
- **连续子数组的最大和**  
```
  public int FindGreatestSumOfSubArray(int[] array) {
    int sum = 0;
    int res = Integer.MIN_VALUE;
    for(int num : array) {
      sum = sum <= 0 ? num : sum + num;
      res = Math.max(res, sum);
     }
     return res;
  }
```
- **二叉树的镜像**  
```
public void Mirror(TreeNode root) {
  if(root == null) {
    return;
  }
  TreeNode tmp = root.left;
  root.left = root.right;
  root.right = tmp;
  // 以时间换空间
  if(root.left != null) {
    Mirror(root.left);  
  }
  if(root.right != null) {
    Mirror(root.right);  
  }
}
```
- **字符流中第一个不重复的字符**    
解题思路：创建一个标记数组记录每个字符的出现次数，创建一个队列，字符流边进边出（当遍历的当前字符重复出现时，出队）
```
Queue<Character> queue = new LinkedList<>();
int[] cnt = new int[128];

public void Insert(char ch) {
  cnt[ch]++；
  queue.add(ch);
  while(!queue.isEmpty() && cnt[queue.peek()] > 1) {
    queue.poll();
  }
}

public char FirstAppearingOnce() {
  return queue.isEmpty() ? '#' : queue.peek(); 
}
```  
- **数组中出现次数超过一半的数字**   
1. 解题思路：“攻守阵地”思想  
2. 代码实现：
```
public int MoreThanHalfNum_Solution(int[] nums) {
  int index = nums[0];
  int cnt = 1;
  for(int i=1; i<nums.length; i++) {
    cnt = nums[i] == index ? cnt+1 : cnt-1;
    if(cnt == 0) {
      index = nums[i];
      cnt = 1;
    }
  }
  cnt = 0;
  for(int num : nums) {
    if(num == index) {
      cnt++;
    }
  }
  return cnt > nums.length/2 ? index : 0;
}
```
- **数组中只出现一次的数字**   
1. 解题思路：一个数和`0`做异或还是它本身，一个数和它本身做异或结果为`0`  
2. 代码实现：  
```
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        int diff = 0;
        for(int num : array){
            diff ^= num;
        }
        int step = 0;
        while((diff&1)==0){
            diff = diff >> 1;
            step++;
        }
        for(int num : array){
            if(((num >> step)&1) == 0){
                num1[0] ^= num;
            }else{
                num2[0] ^= num;
            }
        }
    }
```
- **丑数**  
```
  public int getUglyNumber(int index){
    if(index < 7){
      return index;
    }
    int[] res = new int[index];
    res[0] = 1;
    int p2 = 0, p3 = 0, p5 = 0;
    for(int i = 1;i < index;i++){
      res[i] = Math.min(res[p2] * 2, Math.min(res[p3] * 3, res[p5] * 5));
      if(res[i] == res[p2] * 2){
        p2++;
      }
      if(res[i] == res[p3] * 3){
        p3++;
      }
      if(res[i] == res[p5] * 5){
        p5++;
      }
    }
    return res[index - 1];
  }
```
