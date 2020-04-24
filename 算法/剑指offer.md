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
- **三刷剑指offer**  
```

// 二维数组中的查找
    public boolean Find(int target, int [][] array) {
        int r = 0, c = array[0].length - 1;
        while(r < array.length && c >= 0){
            if(target == array[r][c]){
                return true;
            }else if(target > array[r][c]){
                r++;
            }else{
                c--;
            }
        }
        return false;
    }

// 替换空格
    public String replaceSpace(StringBuffer str) {
        int befo = str.length() - 1;
        for(int i = 0;i <= befo;i++){
            if(str.charAt(i) == ' '){
                str.append("  ");
            }
        }
        int afte = str.length() - 1;
        while(afte > befo){
            char c = str.charAt(befo--);
            if(c == ' '){
                str.setCharAt(afte--, '0');
                str.setCharAt(afte--, '2');
                str.setCharAt(afte--, '%');
            }else{
                str.setCharAt(afte--, c);
            }
        }
        return str.toString();
    }

// 从尾到头打印链表
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ListNode pre = null;
        ListNode cur = listNode;
        ListNode next = null;
        while(cur != null){
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        ArrayList<Integer> res = new ArrayList<>();
        while(pre != null){
            res.add(pre.val);
            pre = pre.next;
        }
        return res;
    }

// 🔺重建二叉树
    HashMap<Integer, Integer> map = new HashMap<>();
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        for(int i = 0;i < in.length;i++){
            map.put(in[i], i);
        }
        return re(pre, 0, pre.length - 1, 0);
    }
    
    public TreeNode re(int[] pre, int preL, int preR, int inL) {
        if(preL > preR){
            return null;
        }
        TreeNode root = new TreeNode(pre[preL]);
        int leftSize = map.get(pre[preL]) - inL;
        root.left = re(pre, preL+1, preL+leftSize, inL);
        root.right = re(pre, preL+leftSize+1, preR, inL+leftSize+1);
        return root;
    }

// 用两个栈实现队列
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    public void push(int node) {
        stack1.push(node);
    }
    
    public int pop() {
        if(stack2.isEmpty()){
            while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }

// 🔺旋转数组的最小数字
    public int minNumberInRotateArray(int [] array) {
        if(array.length == 0){
            return 0;
        }
        int l = 0, r = array.length - 1;
        int m = 0;
        while(l < r){
            m = (l + r) >> 1;
            if(array[m] > array[r]){
                l = m + 1;
            }else if(array[m] < array[r]){
                r = m;
            }else{
                l++;
            }
        }
        return array[l];
    }

// 斐波那契数列
    public int Fibonacci(int n) {
        if(n <= 1){
            return n;
        }
        int pre1 = 0, pre2 = 1;
        int res = 0;
        for(int i = 2;i <= n;i++){
            res = pre1 + pre2;
            pre1 = pre2;
            pre2 = res;
        }
        return res;
    }

// 跳台阶
    public int JumpFloor(int n) {
        if(n <= 3){
            return n;
        }
        int pre1 = 2, pre2 = 3;
        int res = 0;
        for(int i = 4;i <= n;i++){
            res = pre1 + pre2;
            pre1 = pre2;
            pre2 = res;
        }
        return res;
    }

// 变态跳台阶
    public int JumpFloorII(int n) {
        return (int)Math.pow(2, n-1);
    }

// 矩形覆盖
    public int RectCover(int n) {
        if(n <= 3){
            return n;
        }
        int pre1 = 2, pre2 = 3;
        int res = 0;
        for(int i = 4;i <= n;i++){
            res = pre1 + pre2;
            pre1 = pre2;
            pre2 = res;
        }
        return res;
    }

// 二进制中1的个数
    public int NumberOf1(int n) {
        int count = 0;
        while(n != 0){
            count++;
            n &= (n-1);
        }
        return count;
    }

// 数值的整数次方
    public double Power(double base, int exponent) {
        boolean isNega = false;
        if(exponent < 0){
            isNega = true;
            exponent = -exponent;
        }
        double res = 1;
        while(exponent-- > 0){
            res *= base;
        }
        return isNega ? (1/res) : res;
  }

// 调整数组顺序使奇数位于偶数前面
    public void reOrderArray(int [] array) {
        int odd = 0;
        for(int num : array){
            if(num % 2 == 1){
                odd++;
            }
        }
        int[] arr = array.clone();
        int k = 0;
        for(int num : arr){
            if(num % 2 == 0){
                array[odd++] = num;
            }else{
                array[k++] = num;
            }
        }
    }

// 链表中倒数第k个结点
    public ListNode FindKthToTail(ListNode head,int k) {
        ListNode p1 = head;
        while(p1 != null && k-- > 0){
            p1 = p1.next;
        }
        if(k > 0){
            return null;
        }
        ListNode p2 = head;
        while(p1 != null){
            p1 = p1.next;
            p2 = p2.next;
        }
        return p2;
    }

// 反转链表
    public ListNode ReverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        ListNode next = null;
        while(cur != null){
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
```

// 合并两个排序的链表
    public ListNode Merge(ListNode list1,ListNode list2) {
        ListNode list3 = new ListNode(-1);
        ListNode l = list3;
        while(list1 != null && list2 != null){
            if(list1.val < list2.val){
                l.next = list1;
                list1 = list1.next;
            }else{
                l.next = list2;
                list2 = list2.next;
            }
            l = l.next;
        }
        l.next = list1 == null ? list2 : list1;
        return list3.next;
    }
    
// 🔺树的子结构
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        boolean res = false;
        if(root1 != null && root2 != null){
            if(root1.val == root2.val){
                res = sub(root1, root2);
            }
            if(res == false){
                return HasSubtree(root1.left, root2) || HasSubtree(root1.right, root2);
            }
        }
        return res;
    }
    
    public boolean sub(TreeNode node1, TreeNode node2){
        if(node2 == null){
            return true;
        }
        if(node1 == null){
            return false;
        }
        if(node1.val != node2.val){
            return false;
        }
        return sub(node1.left, node2.left) && sub(node1.right, node2.right);
    }
    
// 二叉树的镜像
    public void Mirror(TreeNode root) {
        if(root == null){
            return;
        }
        TreeNode t = root.left;
        root.left = root.right;
        root.right = t;
        Mirror(root.left);
        Mirror(root.right);
    }
    
// 🔺顺时针打印矩阵
    public ArrayList<Integer> printMatrix(int [][] matrix) {
        ArrayList<Integer> res = new ArrayList<>();
        int rb = 0, cb = 0;
        int re = matrix.length - 1, ce = matrix[0].length - 1;
        while(rb <= re && cb <= ce){
            for(int i = cb;i <= ce;i++){
                res.add(matrix[rb][i]);
            }
            for(int i = rb+1;i <= re;i++){
                res.add(matrix[i][ce]);
            }
            if(re > rb){
                for(int i = ce-1;i >= cb;i--){
                    res.add(matrix[re][i]);
                }
            }
            if(ce > cb){
                for(int i = re-1;i > rb;i--){
                    res.add(matrix[i][cb]);
                }
            }
            rb++;
            cb++;
            re--;
            ce--;
        }
        return res;
    }
  
// 包含min函数的栈
    Stack<Integer> s1 = new Stack<>();
    Stack<Integer> s2 = new Stack<>();
    
    public void push(int node) {
        s1.push(node);
        s2.push(s2.isEmpty() ? node : Math.min(node, s2.peek()));
    }
    
    public void pop() {
        s1.pop();
        s2.pop();
    }
    
    public int top() {
        return s1.peek();
    }
    
    public int min() {
        return s2.peek();
    }
    
// 🔺栈的压入、弹出序列
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        Stack<Integer> s = new Stack<>();
        for(int i = 0, j = 0;i < pushA.length;i++){
            s.push(pushA[i]);
            while(!s.isEmpty() && s.peek() == popA[j]){
                s.pop();
                j++;
            }
        }
        return s.isEmpty();
    }
          
// 从上往下打印二叉树
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> res = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        TreeNode t = null;
        while(!q.isEmpty()){
            int cnt = q.size();
            while(cnt-- > 0){
                t = q.poll();
                if(t == null){
                    continue;
                }
                res.add(t.val);
                q.offer(t.left);
                q.offer(t.right);
            }
        }
        return res;
    }
  
// 🔺二叉搜索树的后续遍历序列
    public boolean VerifySquenceOfBST(int [] sequence) {
        if(sequence.length == 0){
            return false;
        }
        return verify(sequence, 0, sequence.length-1);
    }
    
    public boolean verify(int[] sequence, int l, int r){
        if(l >= r){
            return true;
        }
        int cut = l;
        while(cut < r && sequence[cut] < sequence[r]){
            cut++;
        }
        for(int i = cut;i < r;i++){
            if(sequence[i] < sequence[r]){
                return false;
            }
        }
        return verify(sequence, l, cut-1) && verify(sequence, cut, r-1);
    }
