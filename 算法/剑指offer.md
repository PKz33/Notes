## 牛客网
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
    
// 🔺二叉树中和为某一值的路径
    ArrayList<ArrayList<Integer>> res = new ArrayList<>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        find(root, target, new ArrayList<Integer>());
        return res;
    }
    
    public void find(TreeNode node, int target, ArrayList<Integer> cur){
        if(node == null){
            return;
        }
        cur.add(node.val);
        target -= node.val;
        if(target == 0 && node.left == null && node.right == null){
            res.add(new ArrayList(cur));
        }else{
            find(node.left, target, cur);
            find(node.right, target, cur);
        }
        cur.remove(cur.size()-1);
    }
    
// 🔺复杂链表的复制
    public RandomListNode Clone(RandomListNode pHead)
    {
        if(pHead == null){
            return null;
        }
        RandomListNode head = null;
        RandomListNode cur = pHead;
        RandomListNode t = null;
        while(cur != null){
            t = new RandomListNode(cur.label);
            t.next = cur.next;
            cur.next = t;
            cur = t.next;
        }
        cur = pHead;
        while(cur != null){
            t = cur.next;
            if(cur.random != null){
                t.random = cur.random.next;
            }
            cur = t.next;
        }
        head = pHead.next;
        cur = pHead;
        while(cur.next != null){
            t = cur.next;
            cur.next = t.next;
            cur = t;
        }
        return head;
    }
    
// 🔺二叉搜索树与双向链表
    TreeNode head = null;
    TreeNode pre = null;
    public TreeNode Convert(TreeNode pRootOfTree) {
        inOrder(pRootOfTree);
        return head;
    }
    
    public void inOrder(TreeNode node){
        if(node == null){
            return;
        }
        inOrder(node.left);
        if(head == null){
            head = node;
        }
        node.left = pre;
        if(pre != null){
            pre.right = node;
        }
        pre = node;
        inOrder(node.right);
    }
    
// 🔺字符串的排列
    ArrayList<String> l;
    public ArrayList<String> Permutation(String str) {
        l = new ArrayList<>();
        per(str.toCharArray(), 0);
        Collections.sort(l);
        return l;
    }
    
    public void per(char[] c, int idx){
        if(idx == c.length - 1){
            String s = String.valueOf(c);
            if(!l.contains(s)){
                l.add(s);
            }
        }else{
            for(int i = idx, j = i;j < c.length;j++){
                swap(c, i, j);
                per(c, i+1);
                swap(c, i, j);
            }
        }
    }
    
    public void swap(char[] c, int i, int j){
        char t = c[i];
        c[i] = c[j];
        c[j] = t;
    }


// 数组中出现次数超过一半的数字
    public int MoreThanHalfNum_Solution(int [] array) {
        int index = array[0];
        int cnt = 1;
        for(int i = 1;i < array.length;i++){
            cnt = array[i] == index ? cnt + 1 : cnt - 1;
            if(cnt == 0){
                index = array[i];
                cnt = 1;
            }
        }
        cnt = 0;
        for(int num : array){
            if(num == index){
                cnt++;
            }
        }
        return cnt > (array.length >> 1) ? index : 0;
    }
    
// 最小的K个数
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if(k > input.length){
            return new ArrayList<Integer>();
        }
        PriorityQueue<Integer> q = new PriorityQueue<>((o1, o2) -> (o2 - o1));
        for(int n : input){
            q.offer(n);
            if(q.size() > k){
                q.poll();
            }
        }
        return new ArrayList<Integer>(q);
    }
    
// 连续子数组的最大和
    public int FindGreatestSumOfSubArray(int[] array) {
        int sum = 0;
        int res = Integer.MIN_VALUE;
        for(int num : array){
            sum = sum <= 0 ? num : sum + num;
            res = Math.max(res, sum);
        }
        return res;
    }
    
// 🔺整数中1出现的次数（从1到n整数中1出现的次数）
    public int NumberOf1Between1AndN_Solution(int n) {
        int res = 0;
        int a = 0, b = 0;
        for(int i = 1;i <= n;i *= 10){
            a = n / i;
            b = n % i;
            res += (a + 8) / 10 * i + (a % 10 == 1 ? b + 1 : 0); 
        }
        return res;
    }
    
// 🔺把数组排成最小的数
    public String PrintMinNumber(int [] numbers) {
        String[] arr = new String[numbers.length];
        int i = 0;
        for(int num : numbers){
            arr[i++] = String.valueOf(num);
        }
        Arrays.sort(arr, (s1, s2) -> (s1 + s2).compareTo(s2 + s1));
        StringBuilder sb = new StringBuilder();
        for(String s : arr){
            sb.append(s);
        }
        return String.valueOf(sb);
    }
    
// 丑数
    public int GetUglyNumber_Solution(int n) {
        if(n < 7){
            return n;
        }
        int p2 = 0, p3 = 0, p5 = 0;
        int[] res = new int[n];
        res[0] = 1;
        for(int i = 1;i < n;i++){
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
        return res[n - 1];
    }
    
// 第一个只出现一次的字符
    public int FirstNotRepeatingChar(String str) {
        byte[] arr = new byte[128];
        for(char c : str.toCharArray()){
            arr[c]++;
        }
        for(int i = 0;i < str.length();i++){
            if(arr[str.charAt(i)] > 1){
                continue;
            }else{
                return i;
            }
        }
        return -1;
    }
    
// 🔺数组中的逆序对
    int[] t;
    long res;
    public int InversePairs(int [] array) {
        t = new int[array.length];
        res = 0;
        merge(array, 0, array.length - 1);
        return (int)(res % 1000000007);
    }
    
    public void merge(int[] arr, int l, int r){
        if(r <= l){
            return;
        }
        int m = (l + r) >> 1;
        merge(arr, l, m);
        merge(arr, m+1, r);
        merge(arr, l, m, r);
    }
    
    public void merge(int[] arr, int l, int m, int r){
        int i = l, j = m + 1, k = l;
        while(i <= m || j <= r){
            if(i > m){
                t[k] = arr[j++];
            }else if(j > r){
                t[k] = arr[i++];
            }else if(arr[i] <= arr[j]){
                t[k] = arr[i++];
            }else{
                t[k] = arr[j++];
                res += m - i + 1;
            }
            k++;
        }
        for(k = l;k <= r;k++){
            arr[k] = t[k];
        }
    }

// 两个链表的第一个公共结点
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode l1 = pHead1, l2 = pHead2;
        while(l1 != l2){
            l1 = l1 == null ? pHead2 : l1.next;
            l2 = l2 == null ? pHead1 : l2.next;
        }
        return l1;
    }
    
// 数字在排序数组中出现的次数
    // 16ms 9316k
    public int GetNumberOfK(int [] array , int k) {
        int res = 0;
       for(int num : array){
           res = num == k ? res + 1 : res;
       }
        return res;
    }

    // 13ms 9444k
    public int GetNumberOfK(int [] array , int k) {
        return getr(array, k) - getl(array, k) + 1;
    }
    
    public int getl(int[] arr, int k){
        int l = 0, r = arr.length - 1;
        int m = 0;
        while(l <= r){
            m = (l + r) >> 1;
            if(arr[m] < k){
                l = m + 1;
            }else{
                r = m - 1;
            }
        }
        return l;
    }
    
    public int getr(int[] arr, int k){
        int l = 0, r = arr.length - 1;
        int m = 0;
        while(l <= r){
            m = (l + r) >> 1;
            if(arr[m] <= k){
                l = m + 1;
            }else{
                r = m - 1;
            }
        }
        return r;
    }
    
// 二叉树的深度
    public int TreeDepth(TreeNode root) {
        return root == null ? 0 : Math.max(TreeDepth(root.left), TreeDepth(root.right)) + 1;
    }
    
// 平衡二叉树
    public boolean IsBalanced_Solution(TreeNode root) {
        return root == null ? true : Math.abs(depth(root.left) - depth(root.right)) <= 1;
    }
    
    public int depth(TreeNode node){
        return node == null ? 0 : Math.max(depth(node.left), depth(node.right)) + 1;
    }
    
// 🔺数组中只出现一次的数字
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        int diff = 0;
        for(int num : array){
            diff ^= num;
        }
        int step = 0;
        while((diff & 1) == 0){
            diff = diff >> 1;
            step++;
        }
        for(int num : array){
            if(((num >> step) & 1) == 0){
                num1[0] ^= num;
            }else{
                num2[0] ^= num;
            }
        }
    }
    
// 🔺和为S的连续正数序列
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        int l = 1, r = 2;
        int cur = 0;
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        while(l < r){
            cur = (l + r) * (r - l + 1) >> 1;
            if(cur == sum){
                ArrayList<Integer> lis = new ArrayList<>();
                for(int i = l;i <= r;i++){
                    lis.add(i);
                }
                res.add(lis);
                l++;
            }else if(cur < sum){
                r++;
            }else{
                l++;
            }
        }
        return res;
    }
    
// 和为S的两个数字
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        int l = 0, r = array.length - 1;
        int cur = 0;
        ArrayList<Integer> res = new ArrayList<>();
        while(l < r){
            cur = array[l] + array[r];
            if(cur == sum){
                res.add(array[l]);
                res.add(array[r]);
                return res;
            }else if(cur < sum){
                l++;
            }else{
                r--;
            }
        }
        return res;
    }
    
// 左旋转字符串
    public String LeftRotateString(String str,int n) {
        if(str.length() == 0){
            return "";
        }
        n %= str.length();
        StringBuilder sb = new StringBuilder(str);
        sb.append(str);
        return String.valueOf(sb.substring(n, n + str.length()));
    }
    
// 翻转单词顺序列
    public String ReverseSentence(String str) {
        if("".equals(str.trim())){
            return str;
        }
        String[] arr = str.split(" ");
        StringBuilder sb = new StringBuilder();
        for(int i = arr.length - 1;i >= 0;i--){
            sb.append(arr[i]);
            if(i != 0){
                sb.append(" ");
            }
        }
        return String.valueOf(sb);
    }
    
// 🔺扑克牌顺子
    public boolean isContinuous(int [] numbers) {
        if(numbers.length != 5){
            return false;
        }
        byte[] arr = new byte[14];
        int max = -1, min = 14;
        for(int num : numbers){
            if(num == 0){
                continue;
            }
            arr[num]++;
            if(arr[num] > 1){
                return false;
            }
            max = max < num ? num : max;
            min = min > num ? num : min;
        }
        return (max - min) < 5;
    }
    
// 🔺孩子们的游戏（圆圈中最后剩下的数）
    public int LastRemaining_Solution(int n, int m) {
        if(n == 0 || m == 0){
            return -1;
        }
        LinkedList<Integer> l = new LinkedList<>();
        for(int i = 0;i < n;i++){
            l.offer(i);
        }
        int cur = 0;
        while(l.size() > 1){
            cur = (cur + m - 1) % l.size();
            l.remove(cur);
        }
        return l.get(0);
    }
    
// 🔺求1+2+3+...+n
    public int Sum_Solution(int n) {
        int res = n;
        boolean t = (n != 0) && ((res += Sum_Solution(n - 1)) == 666);
        return res;
    }
    
// 🔺不用加减乘除做加法
    public int Add(int num1,int num2) {
        int t = 0;
        while(num1 != 0){
            t = num1 ^ num2;
            num1 = (num1 & num2) << 1;
            num2 = t;
        }
        return num2;
    }
    
// 把字符串转换成整数
    public int StrToInt(String str) {
        if(0 == str.length()){
            return 0;
        }
        boolean isNega = str.charAt(0) == '-' ? true : false;
        int res = 0;
        char c = ' ';
        int cur = 0;
        for(int i = 0;i < str.length();i++){
            c = str.charAt(i);
            if(i == 0 && (c == '-' || c == '+')){
                continue;
            }
            if(c < '0' || c > '9'){
                return 0;
            }
            cur = c - '0';
            if(res == Integer.MAX_VALUE / 10){
                if((isNega == false && cur > 7) || (isNega == true && cur > 8)){
                    return 0;
                }
            }
            res = res * 10 + cur;
        }
        return isNega == true ? -res : res;
    }
    
// 🔺数组中重复的数字
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        int t = 0;
        for(int i = 0;i < length;i++){
            while(numbers[i] != i){
                if(numbers[i] == numbers[numbers[i]]){
                    duplication[0] = numbers[i];
                    return true;
                }
                t = numbers[i];
                numbers[i] = numbers[numbers[i]];
                numbers[t] = t;
            }
        }
        return false;
    }
    
// 🔺构建乘积数组
    public int[] multiply(int[] A) {
        int len = A.length;
        int[] B = new int[len];
        B[0] = 1;
        for(int i = 1;i < len;i++){
            B[i] = B[i - 1] * A[i - 1];
        }
        int t = 1;
        for(int i = len - 1;i >= 0;i--){
            B[i] *= t;
            t *= A[i]; 
        }
        return B;
    }
    
// 🔺正则表达式匹配
    public boolean match(char[] str, char[] pattern)
    {
        return match(str, pattern, 0, 0);
    }
    
    public boolean match(char[] str, char[] pattern, int s, int p){
        if(s == str.length && p == pattern.length){
            return true;
        }
        if(s < str.length && p == pattern.length){
            return false;
        }
        if(p + 1 < pattern.length && pattern[p + 1] == '*'){
            if((s < str.length && pattern[p] == str[s]) || (s < str.length && '.' == pattern[p])){
                return match(str, pattern, s, p + 2)
                    || match(str, pattern, s + 1, p + 2)
                    || match(str, pattern, s + 1, p);
            }else{
                return match(str, pattern, s, p + 2);
            }
        }
        if(s < str.length && ('.' == pattern[p] || str[s] == pattern[p])){
            return match(str, pattern, s + 1, p + 1);
        }
        return false;
    }
    
// 🔺表示数值的字符串
    public boolean isNumeric(char[] str) {
        boolean sig = false, point = false, eE = false;
        for(int i = 0;i < str.length;i++){
            if(str[i] == '+' || str[i] == '-'){
                if(sig && str[i-1] != 'e' && str[i-1] != 'E'){
                    return false;
                }
                if(i > 0 && !sig && str[i-1] != 'e' && str[i-1] != 'E'){
                    return false;
                }
                sig = true;
            }else if(str[i] == 'e' || str[i] == 'E'){
                if(i == str.length-1){
                    return false;
                }
                if(eE){
                    return false;
                }
                eE = true;
            }else if(str[i] == '.'){
                if(eE || point){
                    return false;
                }
                point = true;
            }else if(str[i] < '0' || str[i] > '9'){
                return false;
            }
        }
        return true;
    }
    
// 🔺字符流中第一个不重复的字符
    Queue<Character> q = new LinkedList<>();
    int[] c = new int[128];
    //Insert one char from stringstream
    public void Insert(char ch)
    {
        c[ch]++;
        q.offer(ch);
        while(!q.isEmpty() && c[q.peek()] > 1){
            q.poll();
        }
    }
  //return the first appearence once char in current stringstream
    public char FirstAppearingOnce()
    {
        return q.isEmpty() ? '#' : q.peek();
    }
    
// 链表中环的入口结点
    public ListNode EntryNodeOfLoop(ListNode pHead)
    {
        if(pHead == null || pHead.next == null){
            return null;
        }
        ListNode fast = pHead.next.next, slow = pHead.next;
        while(fast != slow && fast != null ){
            fast = fast.next.next;
            slow = slow.next;
        }
        if(fast == null){
            return null;
        }
        fast = pHead;
        while(fast != slow){
            fast = fast.next;
            slow = slow.next;
        }
        return fast;
    }
    
// 🔺删除链表中重复的结点
    public ListNode deleteDuplication(ListNode pHead)
    {
        ListNode h = new ListNode(-1);
        h.next = pHead;
        ListNode pre = h;
        ListNode cur = h.next;
        while(cur != null){
            if(cur.next != null && cur.val == cur.next.val){
                while(cur.next != null && cur.val == cur.next.val){
                    cur = cur.next;
                }
                pre.next = cur.next;
                cur = cur.next;
            }else{
                pre = cur;
                cur = cur.next;
            }
        }
        return h.next;
    }
    
// 🔺二叉树的下一个结点
    public TreeLinkNode GetNext(TreeLinkNode pNode)
    {
        TreeLinkNode t = null;
        if(pNode.right != null){
            t = pNode.right;
            while(t.left != null){
                t = t.left;
            }
            return t;
        }
        while(pNode.next != null){
            t = pNode.next;
            if(t.left == pNode){
                return t;
            }
            pNode = pNode.next;
        }
        return null;
    }
    
// 🔺对称二叉树
    boolean isSymmetrical(TreeNode pRoot)
    {
        if(pRoot == null){
            return true;
        }
        return sym(pRoot.left, pRoot.right);
    }
    
    boolean sym(TreeNode left, TreeNode right){
        if(left == null && right == null){
            return true;
        }
        if((left == null && right != null) || (right == null && left != null)){
            return false;
        }
        return left.val == right.val && sym(left.left, right.right) && sym(left.right, right.left);
    }
    
// 🔺按之字形顺序打印二叉树
    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        ArrayList<Integer> l = null;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(pRoot);
        int cnt = 0;
        boolean rev = false;
        TreeNode t = null;
        while(!q.isEmpty()){
            l = new ArrayList<>();
            cnt = q.size();
            while(cnt-- > 0){
                t = q.poll();
                if(t == null){
                    continue;
                }
                l.add(t.val);
                q.offer(t.left);
                q.offer(t.right);
            }
            if(rev){
                Collections.reverse(l);
            }
            if(l.size() > 0){
                res.add(l);
            }
            rev = !rev;
        }
        return res;
    }
    
// 把二叉树打印成多行
    ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(pRoot);
        while(!q.isEmpty()){
            ArrayList<Integer> l = new ArrayList<>();
            int c = q.size();
            while(c-- > 0){
                TreeNode t = q.poll();
                if(t == null){
                    continue;
                }
                l.add(t.val);
                q.offer(t.left);
                q.offer(t.right);
            }
            if(l.size() > 0){
                res.add(l);
            }
        }
        return res;
    }
    
// 序列化二叉树
    String dstr = null;
    String Serialize(TreeNode root) {
        if(root == null){
            return "#";
        }
        return root.val + " " + Serialize(root.left) + " " + Serialize(root.right);
  }
    TreeNode Deserialize(String str) {
        if(str.length() == 0){
            return null;
        }
        int idx = str.indexOf(" ");
        String t = idx == -1 ? str : str.substring(0, idx);
        dstr = idx == -1 ? "" : str.substring(idx + 1);
        if("#".equals(t)){
            return null;
        }
        TreeNode tn = new TreeNode(Integer.parseInt(t));
        tn.left = Deserialize(dstr);
        tn.right = Deserialize(dstr);
        return tn;
  }

// 🔺二叉搜索树的第k个结点
    TreeNode KthNode(TreeNode pRoot, int k)
    {
        Stack<TreeNode> s = new Stack<>();
        TreeNode node = pRoot;
        int cnt = 0;
        while(node != null || !s.isEmpty()){
            while(node != null){
                s.push(node);
                node = node.left;
            }
            if(!s.isEmpty()){
                node = s.pop();
                cnt++;
                if(cnt == k){
                    return node;
                }
                node = node.right;
            }
        }
        return null;
    }

// 🔺数据流中的中位数
    PriorityQueue<Integer> p1 = new PriorityQueue<>();
    PriorityQueue<Integer> p2 = new PriorityQueue<>((o1, o2) -> o2 - o1);
    int cnt = 0;
    public void Insert(Integer num) {
        if(cnt % 2 == 0){
            p1.offer(num);
            int t = p1.poll();
            p2.offer(t);
        }else{
            p2.offer(num);
            int t = p2.poll();
            p1.offer(t);
        }
        cnt++;
    }

    public Double GetMedian() {
        return cnt % 2 == 1 ? (double)p2.peek() : ((p1.peek() + p2.peek()) / 2.0);
    }

// 🔺滑动窗口的最大值
    // 190ms 14762k
    public ArrayList<Integer> maxInWindows(int [] num, int size)
    {
        ArrayList<Integer> res = new ArrayList<>();
        if(size < 1 || size > num.length){
            return res;
        }
        PriorityQueue<Integer> q = new PriorityQueue<>((o1, o2) -> o2 - o1);
        for(int i = 0;i < size;i++){
            q.offer(num[i]);
        }
        res.add(q.peek());
        for(int i = 0, j = i + size;j < num.length;i++, j++){
            q.remove(num[i]);
            q.offer(num[j]);
            res.add(q.peek());
        }
        return res;
    }
    
    // 15ms 9300k
    public ArrayList<Integer> maxInWindows(int [] num, int size)
    {
        ArrayList<Integer> res = new ArrayList<>();
        if(size < 1 || size > num.length){
            return res;
        }
        LinkedList<Integer> q = new LinkedList<>();
        for(int i = 0;i < num.length;i++){
            while(!q.isEmpty() && num[q.peekLast()] < num[i]){
                q.pollLast();
            }
            q.addLast(i);
            if(i - size >= q.peekFirst()){
                q.pollFirst();
            }
            if(i - size + 1 >= 0){
                res.add(num[q.peekFirst()]);
            }
        }
        return res;
    }

// 🔺矩阵中的路径
    int[][] dirs = {{0,1}, {0,-1}, {1,0}, {-1,0}};
    public boolean hasPath(char[] matrix, int rows, int cols, char[] str)
    {
        boolean[] flag = new boolean[matrix.length];
        for(int i = 0;i < rows;i++){
            for(int j = 0;j < cols;j++){
                if(bc(matrix, i, j, rows, cols, flag, str, 0)){
                    return true;
                }
            }
        }
        return false;
    }
 
    public boolean bc(char[] matrix, int r, int c, int rows, int cols, boolean[] flag, char[] str, int k){
        int idx = r * cols + c;
        if(r < 0 || r >= rows || c < 0 || c >= cols || flag[idx] == true || matrix[idx] != str[k]){
            return false;
        }
        if(k == str.length - 1){
            return true;
        }
        flag[idx] = true;
        for(int[] num : dirs){
            if(bc(matrix, r + num[0], c + num[1], rows, cols, flag, str, k + 1)){
                return true;
            }
        }
        flag[idx] = false;
        return false;
    }

// 机器人的运动范围
    int[][] dirs;
    int res;
    public int movingCount(int threshold, int rows, int cols)
    {
        dirs = new int[][]{{0,1}, {0,-1}, {1,0}, {-1,0}};
        res = 0;
        boolean[][] mark = new boolean[rows][cols];
        bc(threshold, 0, 0, rows, cols, mark);
        return res;
    }
    
    public void bc(int thr, int r, int c, int rows, int cols, boolean[][] mark){
        int sum = r / 10 + r % 10 + c / 10 + c % 10;
        if(sum > thr || r < 0 || r >= rows || c < 0 || c >= cols || mark[r][c]){
            return;
        }
        mark[r][c] = true;
        res++;
        for(int[] dir : dirs){
            bc(thr, r + dir[0], c + dir[1], rows, cols, mark);
        }
    }

// 剪绳子
    public int cutRope(int n) {
        if(n == 2){
            return 1;
        }
        if(n == 3){
            return 2;
        }
        int a = n / 3, b = n % 3;
        if(b == 0){
            return (int)Math.pow(3, a);
        }else if(b == 1){
            return (int)Math.pow(3, a-1) * 4;
        }else{
            return (int)Math.pow(3, a) * 2;
        }
    }
```
- **最长公共子串**  
```
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String s1 = sc.next();
        String s2 = sc.next();
        int l1 = s1.length();
        int l2 = s2.length();
        int[][] dp = new int[l1+1][l2+1];
        int max = 0;
        int end = 0;
        for(int i = 1;i <= l1;i++){
            for(int j = 1;j <= l2;j++){
                if(s1.charAt(i-1) == s2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1] + 1;
                    if(dp[i][j] > max){
                        max = dp[i][j];
                        end = i;
                    }
                }else{
                    dp[i][j] = 0;
                }
            }
        }
        if(max == 0){
            System.out.println(-1);
        }
        for(int i = (end-max);i < end;i++){
            System.out.print(s1.charAt(i));
        }
        System.out.println();
    }
```
- **字符串排序**  
```
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()){
            String s = sc.next();
            LinkedList<Character> l = new LinkedList<>();
            for(char c : s.toCharArray()){
                if(Character.isLetter(c)){
                    l.offer(c);
                }
            }
            Collections.sort(l, (o1, o2) -> Character.toLowerCase(o1) - Character.toLowerCase(o2));
            int t = 0;
            StringBuilder sb = new StringBuilder();
            for(char c : s.toCharArray()){
                if(Character.isLetter(c) && t < l.size()){
                    sb.append(l.get(t));
                    t++;
                }else{
                    sb.append(c);
                }
            }
            System.out.println(sb.toString());
        }
    }
```
- **任务调度器**  
```
    public int leastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        for(char c : tasks){
            count[c-'A']++;
        }
        int ans = 0;
        Arrays.sort(count);
        while(count[25] > 0){
            int i = 0;
            while(i <= n){
                if(count[25] == 0){
                    break;
                }
                if(i<26 && count[25-i] > 0){
                    count[25-i]--;
                }
                ans++;
                i++;
            }
            Arrays.sort(count);
        }
        return ans;
    }
```
- **找到字符串中所有字母异位词**
```
    public List<Integer> findAnagrams(String s, String p) {
        char[] c1 = p.toCharArray();
        int l1 = p.length();
        Arrays.sort(c1);
        List<Integer> ans = new ArrayList<>();
        for(int i = 0;i < s.length()-l1+1;i++){
            String cur = s.substring(i, i+l1);
            char[] c2 = cur.toCharArray();
            Arrays.sort(c2);
            if(Arrays.equals(c1, c2)){
                ans.add(i);
            }
        }
        return ans;
    }
```
