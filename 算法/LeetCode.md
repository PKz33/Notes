## LeetCode
- **两数之和**  
1. 题目描述：在给定整数数组中查找和为目标值的两个数，返回两数的下标  
2. 代码实现：  
```
  public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for(int i = 0; i < nums.length; i++) {
      if(map.containsKey(target - nums[i]) && map.get(target - nums[i]) != i) {
        return new int[]{i, map.get(target - nums[i])};
      }
      map.put(nums[i], i);
    }
    return new int[]{-1, -1};
  }
```
- **链表的中间结点**  
1. 解题思路：快慢指针（题目中说带有头结点，测试用例却没有带头结点）  
2. 代码实现：  
```
  // Java
  public ListNode middleNode(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while(fast!=null && fast.next!=null){
      slow = slow.next;
      fast = fast.next.next;
    }
    return slow;
  }
  
  # Python
  def middleNode(self, head):
    slow = head
    fast = head
    while fast and fast.next:
      slow = slow.next
      fast = fast.next.next
    return slow
```
- **按摩师**   
1. 解题思路：`dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i])`  
2. 代码实现：  
```
    // Java
    public int massage(int[] nums) {
        int len = nums.length;
        if(len == 0){
            return 0;
        }
        if(len == 1){
            return nums[0];
        }
        int[] dp = new int[len];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for(int i = 2;i < len;i++){
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);
        }
        return dp[len-1];
    }
    
        # Python翻译
        def massage(self, nums):
        if not nums:
            return 0;
        if len(nums) == 1:
            return nums[0]
        dp = [0 for _ in nums]
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])
        for i in range(2, len(nums)):
            dp[i] = max(dp[i-1], dp[i-2] + nums[i])
        return dp[-1]
```
- **三维形体的表面积**  
1. 解题思路：每个网格的立方体增加表面积`个数（高度） * 4 + 2`，然后在遍历过程中，分别减去与已经遍历过的相邻网格中立方体的重叠表面积`Math.min(当前高度, 相邻高度) * 2`
2. 代码实现：  
```
    public int surfaceArea(int[][] grid) {
        int n = grid.length;
        int res = 0;
        for(int i = 0;i < n;i++){
            for(int j = 0;j < n;j++){
                if(grid[i][j] > 0){
                    res += grid[i][j] * 4 + 2;
                    res -= i > 0 ? Math.min(grid[i][j], grid[i-1][j]) * 2 : 0;
                    res -= j > 0 ? Math.min(grid[i][j], grid[i][j-1]) * 2 : 0;
                }
            }
        } 
        return res;      
    }
```
- **买卖股票的最佳时机**  
1. 解题思路：`dp[i] = Math.max(dp[i - 1], prices[i] - min)` 
2. 代码实现：  
```
    public int maxProfit(int[] prices) {
        if(prices.length == 0){
            return 0;
        }
        int min = prices[0];
        int[] dp = new int[prices.length];
        for(int i = 1;i < prices.length;i++){
            dp[i] = Math.max(dp[i-1], prices[i] - min);
            min = Math.min(min, prices[i]);
        }
        return dp[prices.length - 1];
    }

    // 优化
    public int maxProfit(int[] prices) {
        int min = Integer.MAX_VALUE;
        int res = 0;
        for(int i = 0;i < prices.length;i++) {
            res = Math.max(res, prices[i] - min);
            min = Math.min(min, prices[i]);
        }
        return res;
    }
```
- **车的可用捕获量**  
代码实现：  
```
    public int numRookCaptures(char[][] board) {
        int[][] directs = {{0,-1}, {0,1}, {-1,0}, {1,0}};
        int res = 0;
        int x = 0,y = 0;
        for(int i = 0;i < 8;i++){
            for(int j = 0;j < 8;j++){
                if('R' == board[i][j]){
                    x = i;
                    y = j;
                    break;
                }
            }
        }
        for(int k = 0;k < 4;k++){
            int i = x, j = y;
            while(true){
                i += directs[k][0];
                j += directs[k][1];
                if(i <= 0 || i >= 8 || j <= 0 || j >= 8 || 'B' == board[i][j]){
                    break;
                }
                if('p' == board[i][j]){
                    res++;
                    break;
                }
            }
        }
        return res;
    }
```
- **最大子序和**  
1. 解题思路：如果当前`sum < 0`，则重置`sum`的值，因为`sum`为负数和下一个数相加结果只会比当前值小  
2. 代码实现：  
```
    public int maxSubArray(int[] nums) {
        int res = nums[0], sum = 0;
        for(int num : nums){
            if(sum >= 0){
                sum += num;
            }else{
                sum = num;
            }
            res = Math.max(res, sum);
        }
        return res;
    }
```
- **卡牌分组**  
1. 解题思路：统计每种卡牌的次数，判断公约数是否大于等于2  
2. 代码实现：  
```
    public boolean hasGroupsSizeX(int[] deck) {
        if(deck.length < 2) {
            return false;
        }
        int[] count = new int[10000];
        for(int d : deck) {
            count[d]++;
        }
        int x = -1;
        for(int c : count) {
            if(c == 1) {
                return false;
            }
            x = x == -1 ? c : gcd(x, c);
            // 提前终止循环
            if(x == 1) {
                return false;
            }
        }
        return true;
    }

    public int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
```
- **单词的压缩编码**  
1. 解题思路：字典树（后缀所以倒序插入）  
2. 代码实现：  
```
    public int minimumLengthEncoding(String[] words) {
        TireNode root = new TireNode();
        HashMap<TireNode, Integer> map = new HashMap<>();
        for(int i = 0;i < words.length;i++) {
            String str = words[i];
            TireNode cur = root;
            for(int j = str.length() - 1;j >= 0;j--) {
                cur = cur.insert(str.charAt(j));
            }
            map.put(cur, i);
        }
        int res = 0;
        for(TireNode node : map.keySet()) {
            if(node.count == 0) {
                res += words[map.get(node)].length() + 1;
            }
        }
        return res;
    }


class TireNode {
    TireNode[] childs;
    int count;
    public TireNode() {
        childs = new TireNode[26];
        count = 0;
    }
    public TireNode insert(char c) {
        if(childs[c - 'a'] == null) {
            childs[c - 'a'] = new TireNode();
            count++;
        }
        return childs[c - 'a'];
    }
}
```
- **地图分析**  
1. 解题思路：`BFS`，题意有点绕，求每一个海洋区域的最近陆地距离，然后返回这些距离的最大值  
2. 代码实现：
```
    public int maxDistance(int[][] grid) {
        int[][] dir = {{1,0}, {-1,0}, {0,1}, {0,-1}};
        Queue<int[]> q = new LinkedList<>();
        int len = grid.length;
        for(int i = 0;i < len;i++){
            for(int j = 0;j < len;j++){
                if(grid[i][j] == 1){
                    q.offer(new int[]{i, j});
                }
            }
        }
        if(q.isEmpty()){
            return -1;
        }
        boolean hasOcean = false;
        int[] cur = null;
        while(!q.isEmpty()){
            cur = q.poll();
            for(int i = 0;i < 4;i++){
                int x = cur[0] + dir[i][0];
                int y = cur[1] + dir[i][1];
                if(x < 0 || x >= len || y < 0 || y >= len || grid[x][y] != 0){
                    continue;
                }
                hasOcean = true;
                grid[x][y] = grid[cur[0]][cur[1]] + 1;
                q.offer(new int[]{x, y});
            }
        }
        if(!hasOcean){
            return -1;
        }
        return grid[cur[0]][cur[1]] - 1; 
    }
```
- **圆圈中最后剩下的数字**  
1. 解题思路：模拟/数学法
2. 代码实现：  
```
    // 模拟
    public int lastRemaining(int n, int m) {
        ArrayList<Integer> list = new ArrayList<>();
        for(int i = 0;i < n;i++){
            list.add(i);
        }
        int idx = 0;
        while(n > 1){
            idx = (idx + m - 1) % n;
            list.remove(idx);
            n--; 
        }
        return list.get(0);
    }
    
    // 数学法：(当前index + m) % 上一轮剩余数字的个数
    public int lastRemaining(int n, int m) {
        int res = 0;
        for(int i = 2;i <= n;i++){
            res = (res + m) % i;
        }
        return res;
    }
```
- **排序数组**  
![](./Pics/排序.png)  
```
    // 快排
    public int[] sortArray(int[] nums) {
        quickSort(nums, 0, nums.length - 1);
        return nums;
    }

    public void quickSort(int[] nums, int l, int h) {
        if(l < h){
            int idx = partition(nums, l, h);
            quickSort(nums, l, idx - 1);
            quickSort(nums, idx + 1, h);
        }
    }

    public int partition(int[] nums, int l, int h) {
        int t = nums[l];
        while(l < h){
            while(nums[h] >= t && l < h){
                h--;
            }
            nums[l] = nums[h];
            while(nums[l] < t && l < h){
                l++;
            }
            nums[h] = nums[l];
        }
        nums[l] = t;
        return l;
    }


    // 归并排序
    public int[] sortArray(int[] nums) {
        mergeSort(nums, 0, nums.length - 1);
        return nums;
    }

    public void mergeSort(int[] nums, int l, int h) {
        if(l >= h){
            return;
        }
        int mid = (l + h) >> 1;
        mergeSort(nums, l, mid);
        mergeSort(nums, mid + 1, h);
        int[] arr = new int[h - l + 1];
        int s = l, m = mid + 1;
        int k = 0;
        while(s <= mid && m <= h){
            if(nums[s] < nums[m]){
                arr[k++] = nums[s++];
            }else{
                arr[k++] = nums[m++];
            }
        }
        while(s <= mid){
            arr[k++] = nums[s++];
        }
        while(m <= h){
            arr[k++] = nums[m++];
        }
        for(int i = 0;i < arr.length;i++){
            nums[l + i] = arr[i];
        }
    }
    
    
    // 堆排序
    public int[] sortArray(int[] nums) {
        heapSort(nums);
        return nums;
    }

    public void heapSort(int[] nums) {
        int len = nums.length;
        for(int i = (len >> 1) - 1;i >= 0;i--){
            adjust(nums, len, i);
        }
        for(int i = len - 1;i >= 1;i--){
            int t = nums[0];
            nums[0] = nums[i];
            nums[i] = t;
            adjust(nums, i, 0);
        }
    }

    public void adjust(int[] nums, int len, int idx) {
        int l = (idx << 1) + 1;
        int r = (idx << 1) + 2;
        int max = idx;
        if(l < len && nums[l] > nums[max]){
            max = l;
        }
        if(r < len && nums[r] > nums[max]){
            max = r;
        }
        if(max != idx){
            int t = nums[max];
            nums[max] = nums[idx];
            nums[idx] = t;
            adjust(nums, len, max);
        }
    }
```
- **有效括号的嵌套深度**  
1. 题意读了半天没读懂  
2. 代码实现：  
```
    public int[] maxDepthAfterSplit(String seq) {
        int[] res = new int[seq.length()];
        int idx = 0;
        for(char c : seq.toCharArray()){
            res[idx++] = c == '(' ? (idx - 1) & 1 : idx & 1;
        }
        return res;
    }
```
- **有效的括号**  
代码实现：  
```
    public boolean isValid(String s) {
        HashMap<Character, Character> map = new HashMap<>();
        map.put(')','(');
        map.put('}','{');
        map.put(']','[');
        Stack<Character> stack = new Stack<>();
        for(char c : s.toCharArray()){
            if(map.containsKey(c)){
                if(stack.isEmpty()){
                    return false;
                }else{
                    if(map.get(c) != stack.pop()){
                        return false;
                    }
                }                
            }else{
                stack.push(c);
            }
        }
        return stack.isEmpty();     
    }
```
- **生命游戏**  
1. 解题思路：注意状态是同时变化的  
2. 代码实现：  
```
    public void gameOfLife(int[][] board) {
        int[] direct = {-1, 0, 1};
        int m = board.length;
        int n = board[0].length;
        int liveNeighbors;
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                liveNeighbors = 0;
                for(int k = 0;k < 3;k++){
                    for(int l = 0;l < 3;l++){
                        if(!(direct[k] == 0 && direct[l] == 0)){
                            int r = i + direct[k];
                            int c = j + direct[l];
                            if(r >= 0 && r < m && c >= 0 && c < n && (Math.abs(board[r][c]) == 1)){
                                liveNeighbors++;
                            }
                        }
                    }
                }
                if(board[i][j] == 1 && (liveNeighbors < 2 || liveNeighbors > 3)){
                    board[i][j] = -1;
                }
                if(board[i][j] == 0 && liveNeighbors == 3){
                    board[i][j] = 2;
                }
            }
        }
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                board[i][j] = board[i][j] <= 0 ? 0 : 1;
            }
        }
    }
```
- **字符串转换整数（atoi）**  
1. 解题思路：按题意模拟（更简捷的方法用正则表达式）
2. 代码实现：  
```
    public int myAtoi(String str) {
        int res = 0;
        boolean isNegative = false;
        int idx = 0;
        while(idx < str.length() && ' ' == str.charAt(idx)){
            idx++;
        }
        if(idx == str.length()){
            return 0;
        }
        if('-' == str.charAt(idx)){
            isNegative = true;
        }
        if('+' == str.charAt(idx) || '-' == str.charAt(idx)){
            idx++;
        }
        while(idx < str.length() && str.charAt(idx) >= '0' && str.charAt(idx) <= '9'){
            int cur = str.charAt(idx) - '0';
            if(res > Integer.MAX_VALUE / 10 || (res == Integer.MAX_VALUE / 10 && cur >= 8)){
                return isNegative ? Integer.MIN_VALUE : Integer.MAX_VALUE;
            }
            res = res * 10 + cur;
            idx++;
        }
        return isNegative ? -res : res;
    }
```
- **多数元素**  
1. 解题思路：投票算法（攻守阵地算法），注意题目中假定总是存在多数元素  
2. 代码实现：  
```
    public int majorityElement(int[] nums) {
        int res = nums[0];
        int count = 1;
        for(int i = 1;i < nums.length;i++){
            count = nums[i] == res ? count + 1 : count - 1;
            if(count == 0){
                res = nums[i];
                count = 1;
            }
        }
        return res;
    }
    
    // 改写版
    // 代码简捷了一些，但是LC判题后耗时不增反降，是因为创建包装类对象/自动装箱/拆箱？
    public int majorityElement(int[] nums) {
        int count = 0;
        Integer res = null;
        for(int num : nums){
            if(count == 0){
                res = num;
            }
            count += (num == res) ? 1 : -1;
        }
        return res;
    }
```
- **反转链表**  
代码实现：  
```
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null){
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
```
- **用队列实现栈**  
代码实现：
```
class MyStack {
    private Queue<Integer> q;
    /** Initialize your data structure here. */
    public MyStack() {
        q = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        q.add(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        for(int i = 0;i < q.size() - 1;i++){
            q.add(q.poll());
        }
        return q.poll();
    }
    
    /** Get the top element. */
    public int top() {
        for(int i = 0;i < q.size() - 1;i++){
            q.add(q.poll());
        }
        int res = q.poll();
        q.add(res);
        return res;
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q.peek() == null;
    }
}
```
- **接雨水**  
代码实现：
```
    public int trap(int[] height) {
        int res = 0;
        int l = 0, r = height.length - 1;
        int lMax = 0, rMax = 0;
        while(l < r){
            if(height[l] < height[r]){
                if(height[l] >= lMax){
                    lMax = height[l];
                }else{
                    res += lMax - height[l];
                }
                l++;
            }else{
                if(height[r] >= rMax){
                    rMax = height[r];
                }else{
                    res += rMax - height[r];
                }
                r--;
            }
        }
        return res;
    }
```
- **最长回文串**  
1. 解题思路：注意题意是说由给定字符构成的回文串，并不是在给定串中查找  
2. 代码实现：  
```
    public int longestPalindrome(String s) {
        int[] arr = new int[128];
        for(char c : s.toCharArray()){
            arr[c]++;
        }
        int count = 0;
        for(int n : arr){
            count += n % 2;
        }
        return count == 0 ? s.length() : (s.length() - count + 1);
    }
```
- **二叉树的直径**  
1. 解题思路：转换为求二叉树每个节点左右子树高度和的最大值  
2. 代码实现：
```
class Solution {
    int ans;
    public int diameterOfBinaryTree(TreeNode root) {
        // 注意这里
        ans = 1;
        depth(root);
        return ans - 1;
    }

    public int depth(TreeNode root){
        if(root == null){
            return 0;
        }
        int l = depth(root.left);
        int r = depth(root.right);
        ans = Math.max(ans, l + r + 1);
        return Math.max(l, r) + 1;
    }
}
```
- **矩形重叠**  
```
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        return !(rec2[1] >= rec1[3] || rec2[3] <= rec1[1]
            || rec2[2] <= rec1[0] || rec2[0] >= rec1[2]);
    }
```
- **零钱兑换**  
1. 解题思路：自下而上的动态规划  
2. 代码实现：
```
    public int coinChange(int[] coins, int amount) {
        if(amount == 0){
            return 0;
        }
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for(int i = 1;i <= amount;i++){
            for(int j = 0;j < coins.length;j++){
                if(i >= coins[j]){
                    dp[i] = Math.min(dp[i], dp[i-coins[j]] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
```
- **旋转矩阵**  
1. 解题思路：每一组交换的四个数的坐标`(i, j)`，`(j, n-i-1)`，`(n-i-1, n-j-1)`，`(n-j-1, i)`，然后根据方向（顺时针/逆时针）交换
2. 代码实现：  
```
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        int t = matrix[0][0];
        for(int i = 0;i < n / 2;i++){
            for(int j = 0;j < (n + 1) / 2;j++){
                t = matrix[i][j];
                matrix[i][j] = matrix[n-j-1][i];
                matrix[n-j-1][i] = matrix[n-i-1][n-j-1];
                matrix[n-i-1][n-j-1] = matrix[j][n-i-1];
                matrix[j][n-i-1] = t;
            }
        }
    }
```
- **机器人的运动范围**  
1. 解题思路：`DFS`  
2. 代码实现：  
```
    public int movingCount(int m, int n, int k) {
        boolean[][] visited = new boolean[m][n];
        return dfs(0, 0, m, n, k, visited);
    }

    public int dfs(int r, int c, int m, int n, int k, boolean[][] visited){
        if(r<0 || r>=m || c<0 || c>=n || (r%10 + r/10 + c%10 + c/10)>k || visited[r][c]){
            return 0;
        }
        visited[r][c] = true;
        return dfs(r+1, c, m, n, k, visited) + dfs(r-1, c, m, n, k, visited)
            + dfs(r, c+1, m, n, k, visited) + dfs(r, c-1, m, n, k, visited) + 1;
    }
```
- **找树左下角的值**   
```
  public int findBootomLeftValue(TreeNode root){
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while(!queue.isEmpty()){
      root = queue.poll();
      if(root.right != null){
        queue.offer(root.right);
      }
      if(root.left != null){
        queue.offer(root.left)
      }
    }
    return root.val;
  }
```
