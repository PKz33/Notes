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
        ans = 0;
        depth(root);
        return ans;
    }

    public int depth(TreeNode node){
        if(node == null){
            return 0;
        }
        int l = depth(node.left);
        int r = depth(node.right);
        ans = Math.max(ans, l + r);
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
- **盛最多水的容器**  
```
    public int maxArea(int[] height) {
        int l = 0, h = height.length - 1;
        int res = 0;
        while(l < h){
            // 错误写法
            // res = height[l] < height[h] ? Math.max(res, height[l++] * (h - l)) : Math.max(res, height[h--] * (h - l));
            // 正确写法
            res = height[l] < height[h] ? Math.max(res, (h - l) * height[l++]) : Math.max(res, (h - l) * height[h--]);
        }
        return res; 
    }
```
- **火柴拼正方形**  
```
    public boolean makesquare(int[] nums) {
        if(nums.length == 0){
            return false;
        }
        int sum = 0;
        for(int num : nums){
            sum += num;
        }
        if(sum % 4 != 0){
            return false;
        }
        return dfs(nums, 0, new int[4], sum >> 2);
    }

    public boolean dfs(int[] nums, int idx, int[] len, int s){
        if(idx == nums.length){
            if(len[0] == s && len[1] == s && len[2] == s && len[3] == s){
                return true;
            }
        }
        for(int i = 0;i < 4;i++){
            if(len[i] + nums[idx] <= s){
                len[i] += nums[idx];
                if(dfs(nums, idx + 1, len, s)){
                    return true;
                }
                len[i] -= nums[idx];
            }
        }
        return false;
    }
```
- **验证回文字符串Ⅱ**  
```
    public boolean validPalindrome(String s) {
        int l = 0, r = s.length() - 1;
        while(l < r){
            if(s.charAt(l) == s.charAt(r)){
                l++;
                r--;
            }else{
                boolean flag1 = true, flag2 = true;
                for(int i = l, j = r - 1;i < j;i++, j--){
                    if(s.charAt(i) != s.charAt(j)){
                        flag1 = false;
                        break;
                    }
                }
                for(int i = l + 1, j = r;i < j;i++, j--){
                    if(s.charAt(i) != s.charAt(j)){
                        flag2 = false;
                        break;
                    }
                }
                return flag1 || flag2;
            }
        }
        return true;
    }
```
- **朋友圈**  
```
    public int findCircleNum(int[][] M) {
        int[] visited = new int[M.length];
        int count = 0;
        for(int i = 0;i < M.length;i++){
            if(visited[i] == 0){
                dfs(M, visited, i);
                count++;
            }
        }
        return count;
    }

    public void dfs(int[][] M, int[] visited, int i){
        visited[i] = 1;
        for(int j = 0;j < M.length;j++){
            if(M[i][j] == 1 && visited[j] == 0){
                dfs(M, visited, j);
            }
        }
    }
```
- **分饼干**  
```
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int count = 0;
        int gi = 0;
        int si = 0;
        while(gi < g.length && si < s.length){
            if(s[si++] >= g[gi]){
                gi++;
                count++;
            }
        }
        return count;
    }
```
- **长度最小的子数组**  
```
    public int minSubArrayLen(int s, int[] nums) {
        int ans = Integer.MAX_VALUE;
        int sum = 0;
        int l = 0;
        for(int i = 0;i < nums.length;i++){
            sum += nums[i];
            while(sum >= s){
                ans = Math.min(ans, i - l + 1);
                sum -= nums[l++];
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
```
- **三个数的最大乘积**  
```
    public int maximumProduct(int[] nums) {
        int l = nums.length;
        Arrays.sort(nums);
        return Math.max(nums[l - 1] * nums[l - 2] * nums[l - 3], nums[0] * nums[1] * nums[l - 1]);
    }
    
    public int maximumProduct(int[] nums) {
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE;
        int min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
        for(int n : nums){
            if(n > max1){
                max3 = max2;
                max2 = max1;
                max1 = n;
            }else if(n > max2){
                max3 = max2;
                max2 = n;
            }else if(n > max3){
                max3 = n;
            }
            if(n < min1){
                min2 = min1;
                min1 = n;
            }else if(n < min2){
                min2 = n;
            }
        }
        return Math.max(max1 * max2 * max3, max1 * min1 * min2);
    }
```
- **有效的括号**  
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
- **从前序与中序遍历序列构造二叉树**  
```
    HashMap<Integer, Integer> m;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        m = new HashMap<>();
        for(int i = 0;i < inorder.length;i++){
            m.put(inorder[i], i);
        }
        return build(preorder, 0, preorder.length - 1, 0);
    }

    public TreeNode build(int[] pre, int prel, int prer, int inl){
        if(prel > prer){
            return null;
        }
        TreeNode t = new TreeNode(pre[prel]);
        int lsize = m.get(pre[prel]) - inl;
        t.left = build(pre, prel + 1, prel + lsize, inl);
        t.right = build(pre, prel + lsize + 1, prer, inl + lsize + 1);
        return t;
    }
```
- **三数之和**  
```
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        for(int i = 0;i < nums.length - 2;i++){
            if(i > 0 && nums[i] == nums[i - 1]){
                continue;
            }
            int l = i + 1, r = nums.length - 1;
            int sum = 0 - nums[i];
            while(l < r){
                if(nums[l] + nums[r] == sum){
                    ans.add(Arrays.asList(nums[i], nums[l], nums[r]));
                    while(l < r && nums[l] == nums[l + 1]){
                        l++;
                    }
                    while(l < r && nums[r] == nums[r - 1]){
                        r--;
                    }
                    l++;
                    r--;
                }else if(nums[l] + nums[r] < sum){
                    l++;
                }else{
                    r--;
                }
            } 
        }
        return ans;
    }
```  
- **路径总和**  
```
    public boolean hasPathSum(TreeNode root, int sum) {
        return bc(root, sum);
    }

    public boolean bc(TreeNode node, int sum){
        if(node == null){
            return false;
        }
        sum -= node.val;
        if(sum == 0 && node.left == null && node.right == null){
            return true;
        }
        return bc(node.left, sum) || bc(node.right, sum);
    }
```
- **路径总和Ⅱ**  
```
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> ans = new ArrayList<>();
        bc(root, sum, new ArrayList<>(), ans);
        return ans;
    }

    public void bc(TreeNode node, int sum, List<Integer> cur, List<List<Integer>> ans){
        if(node == null){
            return;
        }
        cur.add(node.val);
        if(sum - node.val == 0 && node.left == null && node.right == null){
            ans.add(new ArrayList(cur));
        }
        bc(node.left, sum - node.val, cur, ans);
        bc(node.right, sum - node.val, cur, ans);
        cur.remove(cur.size() - 1);
    }
```
- **除自身以外数组的乘积**  
```
    public int[] productExceptSelf(int[] nums) {
        int[] ans = new int[nums.length];
        int k = 1;
        for(int i = 0;i < nums.length;i++){
            ans[i] = k;
            k *= nums[i];
        }
        k = 1;
        for(int i = nums.length - 1;i >= 0;i--){
            ans[i] *= k;
            k *= nums[i];
        }
        return ans;
    }
```
- **删除链表的倒数第N个节点**  
```
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode p1 = head;
        while(n-- > 0){
            p1 = p1.next;
        }
        if(p1 == null){
            return head.next;
        }
        ListNode p2 = head;
        while(p1.next != null){
            p1 = p1.next;
            p2 = p2.next;
        }
        p2.next = p2.next.next;
        return head;
    }
```  
- **电话号码的字母组合**  
```
    HashMap<String, String> m;
    ArrayList<String> ans;
    public List<String> letterCombinations(String digits) {
        m = new HashMap<>();
        m.put("2", "abc");
        m.put("3", "def");
        m.put("4", "ghi");
        m.put("5", "jkl");
        m.put("6", "mno");
        m.put("7", "pqrs");
        m.put("8", "tuv");
        m.put("9", "wxyz");
        ans = new ArrayList<>();
        if(digits.length() != 0){
            bc("", digits);
        }
        return ans;
    }

    public void bc(String cur, String digs){
        if(digs.length() == 0){
            ans.add(cur);
            return;
        }
        String td = digs.substring(0, 1);
        String ts = m.get(td);
        for(int i = 0;i < ts.length();i++){
            bc(cur + ts.charAt(i), digs.substring(1));
        }
    }
```
- **爬楼梯**  
```
    public int climbStairs(int n) {
        if(n < 3){
            return n;
        }
        int p1 = 1;
        int p2 = 2;
        int sum = 0;
        for(int i = 3;i <= n;i++){
            sum = p1 + p2;
            p1 = p2;
            p2 = sum;
        }
        return sum;
    }
```  
- **对称二叉树**  
```
    public boolean isSymmetric(TreeNode root) {
        if(root == null){
            return true;
        }
        return isSymmetric(root.left, root.right);
    }

    public boolean isSymmetric(TreeNode left, TreeNode right){
        if(left == null && right == null){
            return true;
        }
        if(left == null || right == null){
            return false;
        }
        return left.val == right.val && isSymmetric(left.left, right.right) && isSymmetric(left.right, right.left);
    }
```  
- **二叉树的最大深度**  
```
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
```  
- **环形链表**  
```
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null){
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while(slow != fast){
            if(fast.next == null || fast.next.next == null){
                return false;
            }
            fast = fast.next.next;
            slow = slow.next;
        } 
        return true;
    }
```  
- **最小栈**  
```
    Stack<Integer> s1;
    Stack<Integer> s2;

    public MinStack() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }
    
    public void push(int x) {
        s1.push(x);
        if(s2.isEmpty()){
            s2.push(x);
            return;
        }
        s2.push(s2.peek() < x ? s2.peek() : x);
    }
    
    public void pop() {
        s1.pop();
        s2.pop();
    }
    
    public int top() {
        return s1.peek();
    }
    
    public int getMin() {
        return s2.peek();
    }
```
- **相交链表**  
```
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode l1 = headA;
        ListNode l2 = headB;
        while(l1 != l2){
            l1 = l1 == null ? headB : l1.next;
            l2 = l2 == null ? headA : l2.next;
        }
        return l1; 
    }
```  
- **打家劫舍**  
```
    public int rob(int[] nums) {
        if(nums.length == 0){
            return 0;
        }
        if(nums.length == 1){
            return nums[0];
        }
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for(int i = 2;i < nums.length;i++){
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);
        }
        return dp[nums.length - 1];
    }
```  
- **翻转二叉树**  
```
    public TreeNode invertTree(TreeNode root) {
        if(root == null){
            return null;
        }
        TreeNode t = root.left;
        root.left = root.right;
        root.right = t;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
```  
- **回文链表**  
```
    public boolean isPalindrome(ListNode head) {
        if(head == null){
            return true;
        }
        ListNode firstEnd = endOfHalf(head);
        ListNode secondStart = firstEnd.next;
        ListNode p1 = head;
        ListNode t = reverse(secondStart);
        ListNode p2 = t;
        boolean ans = true;
        while(ans && p2 != null){
            if(p1.val != p2.val){
                ans = false;
            }
            p1 = p1.next;
            p2 = p2.next;
        }
        // firstEnd.next = reverse(t);
        return ans;
    }

    public ListNode endOfHalf(ListNode head){
        ListNode slow = head;
        ListNode fast = head;
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    public ListNode reverse(ListNode node){
        ListNode pre = null;
        ListNode cur = node;
        ListNode next = null;
        while(cur != null){
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;san
        }
        return pre;
    }
```  
- **路径总和III**  
```
    HashMap<Integer, Integer> m;
    public int pathSum(TreeNode root, int sum) {
        m = new HashMap<>();
        m.put(0, 1);
        return bc(root, sum, 0);
    }

    public int bc(TreeNode node, int sum, int path){
        if(node == null){
            return 0;
        }
        path += node.val;
        int t = m.getOrDefault(path - sum, 0);
        m.put(path, m.getOrDefault(path, 0) + 1);
        t += bc(node.left, sum, path) + bc(node.right, sum, path);
        m.put(path, m.get(path) - 1);
        return t;
    }
```
- **找到所有数组中消失的数字**  
```
    public List<Integer> findDisappearedNumbers(int[] nums) {
        HashSet<Integer> s = new HashSet<>();
        for(int n : nums){
            s.add(n);
        }
        List<Integer> ans = new ArrayList<>();
        for(int i = 1;i <= nums.length;i++){
            if(!s.contains(i)){
                ans.add(i);
            }
        }
        return ans;
    }
```  
- **汉明距离**  
```
    public int hammingDistance(int x, int y) {
        int xor = x ^ y;
        int ans = 0;
        while(xor != 0){
            ans++;
            xor &= (xor - 1);
        }
        return ans;
    }
```  
- **把二叉搜索树转换为累加树**  
```
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root != null){
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }
        return root;
    }
```  
- **最短无序连续子数组**  
```
    public int findUnsortedSubarray(int[] nums) {
        Stack<Integer> s = new Stack<>();
        int l = nums.length - 1, r = 0;
        for(int i = 0;i < nums.length;i++){
            while(!s.isEmpty() && nums[s.peek()] > nums[i]){
                l = Math.min(l, s.pop());
            }
            s.push(i);
        }
        s.clear();
        for(int i = nums.length - 1;i >= 0;i--){
            while(!s.isEmpty() && nums[s.peek()] < nums[i]){
                r = Math.max(r, s.pop());
            }
            s.push(i);
        }
        return r - l > 0 ? r - l + 1 : 0;
    }
```  
- **合并二叉树**  
```
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null){
            return t2;
        }
        if(t2 == null){
            return t1;
        }
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
    }
```
- **顺时针打印矩阵**  
```
    public int[] spiralOrder(int[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0){
            return new int[0];
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int[] ans = new int[m * n];
        int l = 0, r = n - 1, t = 0, b = m - 1, k = 0;
        while(l <= r && t <= b){
            for(int col = l;col <= r;col++){
                ans[k++] = matrix[t][col];
            }
            for(int row = t + 1;row <= b;row++){
                ans[k++] = matrix[row][r];
            }
            if(l < r && t < b){
                for(int col = r - 1;col >= l;col--){
                    ans[k++] = matrix[b][col];
                }
                for(int row = b - 1;row > t;row--){
                    ans[k++] = matrix[row][l];
                }
            }
            l++;
            r--;
            t++;
            b--;
        }
        return ans; 
    }
```
- **最长连续序列**  
```
    public int longestConsecutive(int[] nums) {
        HashSet<Integer> s = new HashSet<>();
        for(int num : nums){
            s.add(num);
        }
        int ans = 0;
        for(int num : nums){
            if(!s.contains(num - 1)){
                int cur = 1;
                int curNum = num;
                while(s.contains(curNum + 1)){
                    curNum++;
                    cur++;
                }
                ans = Math.max(cur, ans);
            }
        }
        return ans;
    }
```
- **寻找峰值**  
```
    public int findPeakElement(int[] nums) {
        int l = 0, r = nums.length - 1;
        while(l < r){
            int m = (l + r) >> 1;
            if(nums[m] < nums[m + 1]){
                l = m + 1;
            }else{
                r = m;
            }
        }
        return l;
    }
```  
- **反转字符串中的单词III**  
```
    public String reverseWords(String s) {
        String[] arr = s.split(" ");
        StringBuilder sb = new StringBuilder();
        for(int i = 0;i < arr.length;i++){
            sb.append(new StringBuilder(arr[i]).reverse());
            if(i != arr.length - 1){
                sb.append(" ");
            }
        }
        return sb.toString();
    }
```  
- **数组中的第K个最大元素**  
```
    public int findKthLargest(int[] nums, int k) {
        int l = 0, r = nums.length - 1;
        int tar = nums.length - k;
        while(l <= r){
            int t = part(nums, l, r);
            if(t == tar){
                return nums[tar];
            }else if(t < tar){
                l = t + 1;
            }else{
                r = t - 1;
            }
        }
        return -1;
    }

    public int part(int[] nums, int l, int r){
        int t = nums[l];
        while(l < r){
            while(l < r && nums[r] >= t){
                r--;
            }
            nums[l] = nums[r];
            while(l < r && nums[l] < t){
                l++;
            }
            nums[r] = nums[l];
        }
        nums[l] = t;
        return l;
    }
```
- **字符串转换整数（atoi）**  
```
    public int myAtoi(String str) {
        boolean isNeg = false;
        int idx = 0;
        while(idx < str.length() && ' ' == str.charAt(idx)){
            idx++;
        } 
        if(idx == str.length()){
            return 0;
        }
        if('-' == str.charAt(idx)){
            isNeg = true;
        } 
        if('-' == str.charAt(idx) || '+' == str.charAt(idx)){
            idx++;
        }
        int ans = 0;
        while(idx < str.length() && str.charAt(idx) >= '0' && str.charAt(idx) <= '9'){
            int t = str.charAt(idx) - '0';
            if(ans > Integer.MAX_VALUE / 10 || (ans == Integer.MAX_VALUE / 10 && t > 7)){
                return isNeg ? Integer.MIN_VALUE : Integer.MAX_VALUE;
            }
            ans = ans * 10 + t;
            idx++;
        }
        return isNeg ? -ans : ans;
    }
```
- **无重复字符的最长子串**  
```
    public int lengthOfLongestSubstring(String s) {
        int ans = 0;
        int l = 0;
        HashMap<Character, Integer> m = new HashMap<>();
        for(int i = 0;i < s.length();i++){
            if(m.containsKey(s.charAt(i))){
                l = Math.max(l, m.get(s.charAt(i)) + 1);
            }
            m.put(s.charAt(i), i);
            ans = Math.max(ans, i - l + 1);
        }
        return ans;
    }
```
- **排序链表**  
```
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode slow = head, fast = head.next;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode t = slow.next;
        slow.next = null;
        ListNode l = sortList(head);
        ListNode r = sortList(t);
        ListNode h = new ListNode(-1);
        ListNode ans = h;
        while(l != null && r != null){
            if(l.val < r.val){
                h.next = l;
                l = l.next;
            }else{
                h.next = r;
                r = r.next;
            }
            h = h.next;
        }
        h.next = l == null ? r : l;
        return ans.next;
    }
```
- **回文数**  
```
    public boolean isPalindrome(int x) {
        if(x < 0){
            return false;
        }
        String t = String.valueOf(x);
        int l = 0, r = t.length() - 1;
        while(l < r){
            if(t.charAt(l) != t.charAt(r)){
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
    
    public boolean isPalindrome(int x) {
        if(x < 0){
            return false;
        }
        int t = 1;
        while(x / t >= 10 ){
            t *= 10;
        }
        while(x > 0){
            if(x / t != x % 10){
                return false;
            }
            x = x % t / 10;
            t /= 100;
        }
        return true;
    }
```
- **最长回文子串**  
```
    public String longestPalindrome(String s) {
        if(s.length() < 2){
            return s;
        }
        String ans = s.substring(0, 1);
        int len = 1;
        for(int i = 0;i < s.length() - 1;i++){
            String s1 = centerSpread(s, i, i);
            String s2 = centerSpread(s, i, i + 1);
            String s3 = s1.length() > s2.length() ? s1 : s2;
            if(s3.length() > len){
                len = s3.length();
                ans = s3;
            }
        }
        return ans;
    }

    public String centerSpread(String s, int l, int r){
        while(l >= 0 && r < s.length()){
            if(s.charAt(l) != s.charAt(r)){
                break;
            }
            l--;
            r++;
        }
        return s.substring(l + 1, r);
    }
```
- **寻找两个正序数组的中位数**  
```
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        int len = len1 + len2;
        int p1 = 0, p2 = 0;
        int l = 0, r = 0;
        for(int i = 0;i <= (len >> 1);i++){
            l = r;
            if(p1 < len1 && (p2 >= len2 || nums1[p1] < nums2[p2])){
                r = nums1[p1++];
            }else{
                r = nums2[p2++];
            }
        }
        return (len & 1) == 0 ? (l + r) / 2.0 : r; 
    }
```
- **最长公共前缀**  
```
    public String longestCommonPrefix(String[] strs) {
        if(strs.length == 0){
            return "";
        }
        String s = strs[0];
        for(int i = 1;i < strs.length;i++){
            int j = 0;
            while(j < s.length() && j < strs[i].length()){
                if(s.charAt(j) != strs[i].charAt(j)){
                    break;
                }
                j++;
            }
            s = s.substring(0, j);
            if("".equals(s)){
                return s;
            } 
        }
        return s;
    }
```
- **最长公共子序列**  
```
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        if(m == 0 || n == 0){
            return 0;
        }
        int[][] dp = new int[m+1][n+1];
        for(int i = 1;i <= m;i++){
            for(int j = 1;j <= n;j++){
                dp[i][j] = text1.charAt(i-1) == text2.charAt(j-1) ? dp[i-1][j-1] + 1 : Math.max(dp[i][j-1], dp[i-1][j]);
            }
        }
        return dp[m][n];
    }
```
- **二叉树的序列化与反序列化**  
```
    String str;
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null){
            return "#";
        }
        return root.val + " " + serialize(root.left) + " " + serialize(root.right);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data.length() == 0){
            return null;
        }
        int idx = data.indexOf(" ");
        String t = idx == -1 ? data : data.substring(0, idx);
        str = idx == -1 ? "" : data.substring(idx + 1);
        if(t.equals("#")){
            return null;
        }
        TreeNode node = new TreeNode(Integer.valueOf(t));
        node.left = deserialize(str);
        node.right = deserialize(str);
        return node;
    }
```
- **合并K个排序链表**  
```
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists == null || lists.length == 0){
            return null;
        }
        PriorityQueue<ListNode> q = new PriorityQueue<>((o1, o2) -> o1.val - o2.val);
        ListNode ans = new ListNode(0);
        ListNode p = ans;
        for(ListNode node : lists){
            if(node != null){
                q.offer(node);
            }
        }
        while(!q.isEmpty()){
            p.next = q.poll();
            p = p.next;
            if(p.next != null){
                q.offer(p.next);
            }
        }
        return ans.next;
    }
```
- **环形链表II**  
```
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next == null){
            return null;
        }
        ListNode slow = head.next;
        ListNode fast = head.next.next;
        while(fast != null && fast.next != null && slow != fast){
            slow = slow.next;
            fast = fast.next.next;
        }
        if(fast == null || fast.next == null){
            return null;
        }
        fast = head;
        while(slow != fast){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
```
- **LRU缓存机制**  
```
class Node {
    int k, v;
    Node pre, next;
    public Node(int k, int v){
        this.k = k;
        this.v = v;
    }
}

class DoubleList {
    private Node head, tail;
    private int size;

    public DoubleList(){
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.pre = head;
        size = 0;
    }

    public void addFirst(Node x){
        x.next = head.next;
        head.next.pre = x;
        x.pre = head;
        head.next = x;
        size++;
    }

    public Node removeLast(){
        if(head.next == tail){
            return null;
        }
        Node last = tail.pre;
        remove(last);
        return last;
    }

    public void remove(Node x){
        x.pre.next = x.next;
        x.next.pre = x.pre;
        size--;
    }

    public int size(){
        return size;
    }
}

class LRUCache {
    private HashMap<Integer, Node> m;
    private DoubleList cache;
    private int cap;

    public LRUCache(int capacity) {
        cap = capacity;
        m = new HashMap<>();
        cache = new DoubleList();
    }
    
    public int get(int key) {
        if(!m.containsKey(key)){
            return -1;
        }
        int val = m.get(key).v;
        put(key, val);
        return val;
    }
    
    public void put(int key, int value) {
        Node cur = new Node(key, value);
        if(m.containsKey(key)){
            cache.remove(m.get(key));
            m.put(key, cur);
            cache.addFirst(cur);
        }else{
            if(cap == cache.size()){
                Node last = cache.removeLast();
                m.remove(last.k);
            }
            cache.addFirst(cur);
            m.put(key, cur);
        }
    }
}
```
- **乘积最大子数组**  
```
    public int maxProduct(int[] nums) {
        int ans = Integer.MIN_VALUE, max = 1, min = 1;
        for(int n : nums){
            if(n < 0){
               int t = max;
               max = min;
               min = t; 
            }
            max = Math.max(max * n, n);
            min = Math.min(min * n, n);
            ans = Math.max(ans, max);
        }
        return ans;
    }
```
- **课程表**  
```
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] indgs = new int[numCourses];
        List<List<Integer>> graph = new ArrayList<>();
        Queue<Integer> q = new LinkedList<>();
        for(int i = 0;i < numCourses;i++){
            graph.add(new ArrayList<>());
        }
        for(int[] p : prerequisites){
            indgs[p[0]]++;
            graph.get(p[1]).add(p[0]);
        }
        for(int i = 0;i < numCourses;i++){
            if(indgs[i] == 0){
                q.offer(i);
            }
        }
        while(!q.isEmpty()){
            int pre = q.poll();
            numCourses--;
            for(int n : graph.get(pre)){
                indgs[n]--;
                if(indgs[n] == 0){
                    q.offer(n);
                }
            }
        }
        return numCourses == 0;
    }
```
- **实现 Trie (前缀树)**  
```
class Trie {
    TireNode root;

    /** Initialize your data structure here. */
    public Trie() {
        root = new TireNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TireNode node = root;
        for(int i = 0;i < word.length();i++){
            char c = word.charAt(i);
            if(!node.containsKey(c)){
                node.put(c, new TireNode());
            }
            node = node.get(c);
        }
        node.setEnd();
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TireNode node = searchPrefix(word);
        return node != null && node.isEnd();
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        return searchPrefix(prefix) != null;
    }

    public TireNode searchPrefix(String word){
        TireNode node = root;
        for(int i = 0;i < word.length();i++){
            char c = word.charAt(i);
            if(node.containsKey(c)){
                node = node.get(c);
            }else{
                return null;
            }
        }
        return node;
    }
}

class TireNode {
    TireNode[] link;
    final int R = 26;
    boolean isEnd;
    
    public TireNode(){
        link = new TireNode[R];
    }

    public boolean containsKey(char c){
        return link[c - 'a'] != null;
    }

    public TireNode get(char c){
        return link[c - 'a'];
    }

    public void put(char c, TireNode node){
        link[c - 'a'] = node;
    }

    public void setEnd(){
        isEnd = true;
    }

    public boolean isEnd(){
        return isEnd;
    }
}
```
- **每日温度**  
```
    public int[] dailyTemperatures(int[] T) {
        int[] ans = new int[T.length];
        Stack<Integer> s = new Stack<>();
        for(int i = 0;i < T.length;i++){
            while(!s.isEmpty() && T[i] > T[s.peek()]){
                int t = s.pop();
                ans[t] = i - t;
            }
            s.push(i);
        }
        return ans;
    }
```
- **搜索旋转排序数组**  
```
    public int search(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        while(l <= r){
            int m = (l + r) >> 1;
            if(target == nums[m]){
                return m;
            }
            if(nums[l] <= nums[m]){
                if(nums[l] <= target && target < nums[m]){
                    r = m - 1;
                }else{
                    l = m + 1;
                }
            }else{
                if(nums[m] < target && target <= nums[r]){
                    l = m + 1;
                }else{
                    r = m - 1;
                }
            }
        }
        return -1;
    }
```
- **最长连续递增序列**  
```
    public int findLengthOfLCIS(int[] nums) {
        int ans = 0, t = 0;
        for(int i = 0;i < nums.length;i++){
            if(i > 0 && nums[i-1] >= nums[i]){
                t = i;
            }
            ans = Math.max(ans, i - t + 1);
        }
        return ans;
    }
```  
- **最长上升子序列**  
```
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 0){
            return 0;
        }
        int[] dp = new int[nums.length];
        dp[0] = 1;
        int ans = 1;
        for(int i = 1;i < nums.length;i++){
            int max = 0;
            for(int j = 0;j < i;j++){
                if(nums[i] > nums[j]){
                    max = Math.max(max, dp[j]);
                }
            }
            dp[i] = max + 1;
            ans = Math.max(ans, dp[i]);
        }
        return ans;
    }
```
- **回文子串**  
```
    public int countSubstrings(String s) {
        int ans = 0;
        int len = s.length();
        for(int i = 0;i < len;i++){
            int l = i, r = i;
            while(l >= 0 && r < len && s.charAt(l) == s.charAt(r)){
                ans++;
                l--;
                r++;
            }
            l = i; 
            r = i + 1;
            while(l >= 0 && r < len && s.charAt(l) == s.charAt(r)){
                ans++;
                l--;
                r++;
            }
        }
        return ans;
    }
```
- **比特位计数**  
```
    public int[] countBits(int num) {
        int[] ans = new int[num + 1];
        int i = 0, b = 1;
        while(b <= num){
            while(i < b && i+b <= num){
                ans[i+b] = ans[i] + 1;
                i++;
            }
            i = 0;
            b = b << 1;
        }
        return ans;
    }
```  
- **全排列**  
```
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> l = new ArrayList<>();
        for(int num : nums){
            l.add(num);
        }
        bc(l, ans, 0, nums.length);
        return ans;
    }

    public void bc(List<Integer> l, List<List<Integer>> ans, int idx, int n){
        if(idx == n){
            ans.add(new ArrayList(l));
        }
        for(int i = idx;i < n;i++){
            Collections.swap(l, idx, i);
            bc(l, ans, idx + 1, n);
            Collections.swap(l, idx, i);
        }
    }
```
- **二叉树展开为链表**  
```
    public void flatten(TreeNode root) {
        while(root != null){
            if(root.left == null){
                root = root.right;
            }else{
                TreeNode t = root.left;
                while(t.right != null){
                    t = t.right;
                }
                t.right = root.right;
                root.right = root.left;
                root.left = null;
                root = root.right;
            }
        }
    }
```
- **组合总和**  
```
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(candidates);
        bc(candidates, target, 0, ans, new LinkedList<>());
        return ans;
    }

    public void bc(int[] candidates, int cur, int idx, List<List<Integer>> ans, LinkedList<Integer> path){
        if(cur == 0){
            ans.add(new ArrayList(path));
            return;
        }
        for(int i = idx;i < candidates.length;i++){
            if(cur - candidates[i] < 0){
                break;
            }
            path.offer(candidates[i]);
            bc(candidates, cur-candidates[i], i, ans, path);
            path.removeLast();
        }
    }
```  
- **括号生成**  
```
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        bc(ans, new StringBuilder(), 0, 0, n);
        return ans;
    }

    public void bc(List<String> ans, StringBuilder sb, int l, int r, int n){
        if(sb.length() == (n << 1)){
            ans.add(sb.toString());
            return;
        }
        if(l < n){
            sb.append("(");
            bc(ans, sb, l+1, r, n);
            sb.deleteCharAt(sb.length()-1);
        }
        if(r < l){
            sb.append(")");
            bc(ans, sb, l, r+1, n);
            sb.deleteCharAt(sb.length()-1);
        }
    }
```
- **不同路径**  
```
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for(int i = 0;i < m;i++){
            dp[i][0] = 1;
        }
        for(int i = 0;i < n;i++){
            dp[0][i] = 1;
        }
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
```
- **下一个排列**  
```
    public void nextPermutation(int[] nums) {
        int i = nums.length - 2;
        while(i >= 0 && nums[i+1] <= nums[i]){
            i--;
        }
        if(i >= 0){
            int j = nums.length - 1;
            while(j >= 0 && nums[j] <= nums[i]){
                j--;
            }
            swap(nums, i, j);
        }
        reverse(nums, i+1);
    }

    public void swap(int[] nums, int i, int j){
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

    public void reverse(int[] nums, int i){
        int j = nums.length - 1;
        while(i < j){
            swap(nums, i, j);
            i++;
            j--;
        }
    }
```
- **二叉树的最近公共祖先**  
```
    TreeNode ans = null;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        dfs(root, p, q);
        return ans;
    }

    public boolean dfs(TreeNode root, TreeNode p, TreeNode q){
        if(root == null){
            return false;
        }
        boolean l = dfs(root.left, p, q);
        boolean r = dfs(root.right, p, q);
        if((l && r) || ((root.val == p.val || root.val == q.val) && (l || r))){
            ans = root;
        }
        return (l || r) || (root.val == p.val || root.val == q.val); 
    }
```
- **最佳买卖股票时机含冷冻期**  
```
    public int maxProfit(int[] prices) {
        if(prices.length == 0){
            return 0;
        }
        int n = prices.length;
        int[][] dp = new int[n][3];
        dp[0][0] = -prices[0];
        for(int i = 1;i < n;i++){
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][2] - prices[i]);
            dp[i][1] = dp[i-1][0] + prices[i];
            dp[i][2] = Math.max(dp[i-1][1], dp[i-1][2]);
        }
        return Math.max(dp[n-1][1], dp[n-1][2]);
    }
```
- **打家劫舍III**  
```
    public int rob(TreeNode root) {
        HashMap<TreeNode, Integer> m = new HashMap<>();
        return rob(root, m);
    }

    public int rob(TreeNode node, HashMap<TreeNode, Integer> m){
        if(node == null){
            return 0;
        }
        if(m.containsKey(node)){
            return m.get(node);
        }
        int cur = node.val;
        if(node.left != null){
            cur += rob(node.left.left, m) + rob(node.left.right, m);
        }
        if(node.right != null){
            cur += rob(node.right.left, m) + rob(node.right.right, m);
        }
        int ans = Math.max(cur, rob(node.left, m) + rob(node.right, m));
        m.put(node, ans);
        return ans;
    }
```
- **搜索二维矩阵II**
```
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix.length == 0 || matrix[0].length == 0){
            return false;
        }
        int i = 0, j = matrix[0].length - 1;
        while(i < matrix.length && j >= 0){
            if(matrix[i][j] == target){
                return true;
            }else if(matrix[i][j] < target){
                i++;
            }else{
                j--;
            }
        }
        return false;
    }
```  
- **最大正方形**  
```
    public int maximalSquare(char[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0){
            return 0;
        }
        int t = 0;
        int[][] dp = new int[matrix.length][matrix[0].length];
        for(int i = 0;i < matrix.length;i++){
            for(int j = 0;j < matrix[0].length;j++){
                if(matrix[i][j] == '1'){
                    if(i == 0 || j == 0){
                        dp[i][j] = 1;
                    }else{
                        dp[i][j] = Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1])) + 1;
                    }
                    t = Math.max(t, dp[i][j]);
                }
            }
        }
        return t * t;
    }
```
- **完全平方数**  
```
    public int numSquares(int n) {
        int[] dp = new int[n+1];
        for(int i = 1;i <= n;i++){
            dp[i] = i;
            for(int j = 1;i-j*j >= 0;j++){
                dp[i] = Math.min(dp[i], dp[i-j*j] + 1);
            }
        }
        return dp[n];
    }
```
- **在排序数组中查找元素的第一个和最后一个位置**
```
    public int[] searchRange(int[] nums, int target) {
        int[] ans = new int[2];
        ans[0] = getStart(nums, target);
        ans[1] = getEnd(nums, target);
        return ans;
    }

    public int getStart(int[] nums, int target){
        int l = 0, r = nums.length - 1;
        while(l <= r){
            int m = (l + r) >> 1;
            if(nums[m] >= target){
                r = m-1;
            }else{
                l = m+1;
            }
        }
        return (l >= nums.length || nums[l] != target)? -1 : l;
    }

    public int getEnd(int[] nums, int target){
        int l = 0, r = nums.length - 1;
        while(l <= r){
            int m = (l + r) >> 1;
            if(nums[m] <= target){
                l = m+1;
            }else{
                r = m-1;
            }
        }
        return (r < 0 || nums[r] != target)? -1 : r;
    }
```
- **旋转图像**  
```
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for(int i = 0;i < n;i++){
            for(int j = i;j < n;j++){
                int t = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = t;
            }
        }
        for(int i = 0;i < n;i++){
            for(int j = 0;j < (n >> 1);j++){
                int t = matrix[i][j];
                matrix[i][j] = matrix[i][n-j-1];
                matrix[i][n-j-1] = t;
            }
        }
    }
```
- **字母异位词分组**  
```
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List> ans = new HashMap<>();
        int[] count = new int[26];
        for(String s : strs){
            Arrays.fill(count, 0);
            for(char c : s.toCharArray()){
                count[c-'a']++;
            }
            StringBuilder sb = new StringBuilder();
            for(int cnt : count){
                sb.append(cnt);
            }
            String key = sb.toString();
            if(!ans.containsKey(key)){
                ans.put(key, new ArrayList<>());
            }
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }
```  
- **最长公共子序列**  
```
    public int longestCommonSubsequence(String text1, String text2) {
        int l1 = text1.length(), l2 = text2.length();
        int[][] dp = new int[l1+1][l2+1];
        for(int i = 1;i <= l1;i++){
            for(int j = 1;j <= l2;j++){
                dp[i][j] = text1.charAt(i-1) == text2.charAt(j-1) ? dp[i-1][j-1] + 1 : Math.max(dp[i-1][j], dp[i][j-1]); 
            }
        }
        return dp[l1][l2];
    }
```  
- **回文素数**  
```
    public int primePalindrome(int N) {
        while(true){
            if(reverse(N) == N && isPrime(N)){
                return N;
            }
            N++;
            // 不存在长度为8的素数
            if (10_000_000 < N && N < 100_000_000){
                N = 100_000_000;
            }
        }
    }

    public int reverse(int N){
        int t = 0;
        while(N > 0){
            t = t*10 + N%10;
            N /= 10;
        }
        return t;
    }

    public boolean isPrime(int N){
        if(N < 2){
            return false;
        }
        int t = (int)Math.sqrt(N);
        for(int i = 2;i <= t;i++){
            if(N % i == 0){
                return false;
            }
        }
        return true;
    }
```
- **跳跃游戏**  
```
    public boolean canJump(int[] nums) {
        int max = 0;
        for(int i = 0;i < nums.length;i++){
            if(i <= max){
                max = Math.max(max, i+nums[i]);
                if(max >= nums.length-1){
                    return true;
                }
            }
        }
        return false;
    }
```
- **合并区间**  
```
    public int[][] merge(int[][] intervals) {
        if(intervals.length == 0){
            return new int[0][0];
        }
        int[][] ans = new int[intervals.length][2];
        Arrays.sort(intervals, (o1, o2) -> o1[0] - o2[0]);
        int k = 0;
        ans[k][0] = intervals[0][0];
        ans[k][1] = intervals[0][1];
        for(int i = 1;i < intervals.length;i++){
            int l = ans[k][0], r = ans[k][1];
            if(intervals[i][0] <= r){
                ans[k][1] = Math.max(ans[k][1], intervals[i][1]);
            }else{
                k += 1;
                ans[k][0] = intervals[i][0];
                ans[k][1] = intervals[i][1];
            }
        }
        return Arrays.copyOf(ans, k+1);
    }
```
- **最小路径和**  
```
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] dp = new int[m][n];
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                if(i == 0 && j == 0){
                    dp[i][j] = grid[0][0];
                }else if(i == 0){
                    dp[i][j] = dp[i][j-1] + grid[i][j];
                }else if(j == 0){
                    dp[i][j] = dp[i-1][j] + grid[i][j];
                }else{
                    dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
                }
            }
        }
        return dp[m-1][n-1];
    }
```  
- **颜色分类**  
```
    public void sortColors(int[] nums) {
        int r0 = 0, l2 = nums.length-1, cur = 0;
        while(cur <= l2){
            if(nums[cur] == 0){
                int t = nums[r0];
                nums[r0++] = nums[cur];
                nums[cur++] = t;
            }else if(nums[cur] == 1){
                cur++;
            }else{
                int t = nums[l2];
                nums[l2--] = nums[cur];
                nums[cur] = t;
            }
        }
    }
```
- **子集**  
```
    List<List<Integer>> ans;
    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        ans = new ArrayList<>();
        for(int i = 0;i < n+1;i++){
            bc(0, i, n, new ArrayList<>(), nums);
        }
        return ans;
    }

    public void bc(int id, int k, int n, List<Integer> l, int[] nums){
        if(l.size() == k){
            ans.add(new ArrayList(l));
        }
        for(int i = id;i < n;i++){
            l.add(nums[i]);
            bc(i+1, k, n, l, nums);
            l.remove(l.size()-1);
        }
    } 
```
- **单词搜索**  
```
    int[][] dirts = {{0,1}, {0,-1}, {1,0}, {-1,0}};
    public boolean exist(char[][] board, String word) {
        boolean[][] marked = new boolean[board.length][board[0].length];
        for(int i = 0;i < board.length;i++){
            for(int j = 0;j < board[0].length;j++){
                if(dfs(i, j, board, 0, word, marked)){
                    return true;
                }
            }
        }
        return false;
    }

    public boolean dfs(int i, int j, char[][] board, int cur, String word, boolean[][] marked){
        if(i<0 || i>=board.length || j<0 || j>=board[0].length || board[i][j] != word.charAt(cur) || marked[i][j]){
            return false;
        }
        if(cur == word.length()-1){
            return true;
        }
        marked[i][j] = true;
        for(int[] dir : dirts){
            if(dfs(i+dir[0], j+dir[1], board, cur+1, word, marked)){
                return true;
            }
        }
        marked[i][j] = false;
        return false;
    }
```
- **二叉树的中序遍历**  
```
    public List<Integer> inorderTraversal(TreeNode root) {
        Stack<TreeNode> s = new Stack<>();
        List<Integer> ans = new ArrayList<>();
        TreeNode cur = root;
        while(cur!= null || !s.isEmpty()){
            while(cur != null){
                s.push(cur);
                cur = cur.left;
            }
            cur = s.pop();
            ans.add(cur.val);
            cur = cur.right;
        }
        return ans;
    }
```  
- **验证二叉搜索树**  
```
    public boolean isValidBST(TreeNode root) {
        Stack<TreeNode> s = new Stack<>();
        long pre = Long.MIN_VALUE;
        TreeNode cur = root;
        while(cur != null || !s.isEmpty()){
            while(cur != null){
                s.push(cur);
                cur = cur.left;
            }
            cur = s.pop();
            if(cur.val <= pre){
                return false;
            }
            pre = cur.val;
            cur = cur.right;
        }
        return true;
    }
```
- **不同的二叉搜索树**  
```
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2;i <= n;i++){
            for(int j = 1;j <= i;j++){
                dp[i] += dp[j-1] * dp[i-j];
            }
        }
        return dp[n];
    }
```
- **二叉树的层序遍历**  
```
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            int s = q.size();
            List<Integer> l = new ArrayList<>();
            while(s-- > 0){
                TreeNode t = q.poll();
                if(t == null){
                    continue;
                }
                l.add(t.val);
                q.offer(t.left);
                q.offer(t.right);
            }
            if(l.size() > 0){
                ans.add(l);
            }
        }
        return ans;
    }
```
- **单词拆分**  
```
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> set = new HashSet(wordDict);
        boolean[] dp = new boolean[s.length()+1];
        dp[0] = true;
        for(int i = 1;i <= s.length();i++){
            for(int j = 0;j < i;j++){
                if(dp[j] && set.contains(s.substring(j, i))){
                    dp[i] = true;
                }
            }
        }
        return dp[s.length()];
    }
```
- **岛屿数量**  
```
    int[][] dirs = {{1,0}, {-1,0}, {0,1}, {0,-1}};
    public int numIslands(char[][] grid) {
        int ans = 0;
        if(grid == null || grid.length == 0){
            return ans;
        }
        int rs = grid.length, cs = grid[0].length;
        for(int i = 0;i < rs;i++){
            for(int j = 0;j < cs;j++){
                if(grid[i][j] == '1'){
                    ans++;
                    dfs(grid, i, j, rs, cs);
                }
            }
        }
        return ans;
    }

    public void dfs(char[][] grid, int r, int c, int rs, int cs){
        if(r < 0 || c < 0 || r >= rs || c >= cs || grid[r][c] == '0'){
            return;
        }
        grid[r][c] = '0';
        for(int[] dir : dirs){
            dfs(grid, r+dir[0], c+dir[1], rs, cs);
        }
    }
```
- **寻找重复数**  
```
    public int findDuplicate(int[] nums) {
        int l = 1, r = nums.length - 1;
        while(l < r){
            int t = (l + r) >>> 1;
            int cnt = 0;
            for(int n : nums){
                if(n <= t){
                    cnt++;
                }
            }
            if(cnt > t){
                r = t;
            }else{
                l = t + 1;
            }
        }
        return l;
    }
```
- **前K个高频元素**  
```
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> m = new HashMap<>();
        for(int n : nums){
            if(m.containsKey(n)){
                m.put(n, m.get(n)+1);
            }else{
                m.put(n, 1);
            }
        }
        PriorityQueue<Integer> q = new PriorityQueue<>((o1, o2) -> m.get(o1) - m.get(o2));
        for(int key : m.keySet()){
            if(q.size() < k){
                q.offer(key);
            }else{
                if(m.get(key) > m.get(q.peek())){
                    q.poll();
                    q.offer(key);
                }
            }
        }
        int[] ans = new int[k];
        int t = 0;
        while(!q.isEmpty()){
            ans[t++] = q.poll();
        }
        return ans;
    }
```
- **字符串解码**  
```
    public String decodeString(String s) {
        Stack<String> ss = new Stack<>();
        Stack<Integer> sn = new Stack<>();
        StringBuilder ans = new StringBuilder();
        int t = 0;
        for(char c : s.toCharArray()){
            if(c == '['){
                ss.push(ans.toString());
                sn.push(t);
                ans = new StringBuilder();
                t = 0;
            }else if(c == ']'){
                StringBuilder sb = new StringBuilder();
                int n = sn.pop();
                for(int i = 0;i < n;i++){
                    sb.append(ans);
                }
                ans = new StringBuilder(ss.pop() + sb);
            }else if(c >= '0' && c <= '9'){
                t = t*10 + Integer.parseInt(c+"");
            }else{
                ans.append(c);
            }
        }
        return ans.toString();
    }
```  
- **目标和**  
```
    int[] dirs = {-1, 1};
    int ans;
    public int findTargetSumWays(int[] nums, int S) {
        ans = 0;
        int cur = 0;
        dfs(nums, 0, cur, S);
        return ans;
    }

    public void dfs(int[] nums, int id, int cur, int S){
        if(id == nums.length){
            if(cur == S){
                ans++;
            }
            return;
        }
        for(int d : dirs){
            cur += nums[id]*d;
            dfs(nums, id+1, cur, S);
            cur -= nums[id]*d;
        }
    }
```  
- **根据身高重建队列**  
```
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (o1, o2) -> o1[0] == o2[0] ? o1[1]-o2[1] : o2[0]-o1[0]);
        List<int[]> l = new LinkedList<>();
        for(int[] p : people){
            l.add(p[1], p);
        }
        int n = people.length;
        return l.toArray(new int[n][2]);
    }
```  
- **分割等和子集**  
```
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int num : nums){
            sum += num;
        }
        if(sum%2 != 0){
            return false;
        }
        sum = (sum>>>1);
        int n = nums.length;
        boolean[][] dp = new boolean[n][sum+1];
        if(nums[0] <= sum){
            dp[0][nums[0]] = true;
        }
        for(int i = 1;i < n;i++){
            for(int j = 0;j <= sum;j++){
                if(nums[i] > j){
                    dp[i][j] = dp[i-1][j];
                }else if(nums[i] == j){
                    dp[i][j] = true;
                }else{
                    dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i]];
                }
            }
        }
        return dp[n-1][sum];
    }
```
- **和为K的子数组**  
```
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> m = new HashMap<>();
        m.put(0, 1);
        int pre = 0;
        int ans = 0;
        for(int n : nums){
            pre += n;
            if(m.containsKey(pre-k)){
                ans += m.get(pre-k);
            }
            m.put(pre, m.getOrDefault(pre, 0) + 1);
        }
        return ans;
    }
```
- **二叉搜索树的最近公共祖先**  
```
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        int rval = root.val;
        int pval = p.val;
        int qval = q.val;
        if(pval>rval && qval>rval){
            return lowestCommonAncestor(root.right, p, q);
        }else if(pval<rval && qval<rval){
            return lowestCommonAncestor(root.left, p, q);
        }else{
            return root;
        }
    }
```
- **三角形最小路径和**  
```
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] dp = new int[n][n];
        dp[0][0] = triangle.get(0).get(0);
        for(int i = 1;i < n;i++){
            dp[i][0] = dp[i-1][0] + triangle.get(i).get(0);
            for(int j = 1;j < i;j++){
                dp[i][j] = Math.min(dp[i-1][j-1], dp[i-1][j]) + triangle.get(i).get(j);
            }
            dp[i][i] = dp[i-1][i-1] + triangle.get(i).get(i);
        }
        int ans = dp[n-1][0];
        for(int i = 1;i < n;i++){
            ans = Math.min(ans, dp[n-1][i]);
        }
        return ans;
    }
```
- **字符串的排列**  
```
    public boolean checkInclusion(String s1, String s2) {
        if(s1.length() > s2.length()){
            return false;
        }
        int[] t1 = new int[26];
        for(char c : s1.toCharArray()){
            t1[c-'a']++;
        }
        for(int i = 0;i <= s2.length()-s1.length();i++){
            int[] t2 = new int[26];
            for(int j = i;j < i+s1.length();j++){
                t2[s2.charAt(j)-'a']++;
            }
            if(match(t1, t2)){
                return true;
            }
        }
        return false;
    }

    public boolean match(int[] t1, int[] t2){
        for(int i = 0;i < 26;i++){
            if(t1[i] != t2[i]){
                return false;
            }
        }
        return true;
    }
```  
- **二叉树的锯齿形层次遍历**  
```
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        LinkedList<TreeNode> l = new LinkedList<>();
        l.offer(root);
        boolean flag = true;
        while(!l.isEmpty()){
            int size = l.size();
            TreeNode cur = null;
            ArrayList<Integer> lt = new ArrayList<>();
            while(size-- > 0){
                cur = l.poll();
                if(cur == null){
                    continue;
                }
                lt.add(cur.val);
                l.offer(cur.left);
                l.offer(cur.right);
            }
            if(flag == false){
                Collections.reverse(lt);
            }
            if(lt.size() > 0){
                ans.add(lt);
            }
            flag = !flag;
        }
        return ans;
    }
```
- **翻转字符串里的单词**  
```
    public String reverseWords(String s) {
        s = s.trim();
        String[] arr = s.split("\\s+");
        StringBuilder sb = new StringBuilder();
        for(int i = arr.length-1;i >= 0;i--){
            if(i == 0){
                sb.append(arr[i]);
            }else{
                sb.append(arr[i] + " ");
            }
        }
        return sb.toString();
    }
```
- **岛屿的最大面积**  
```
    int ans;
    int cur;
    int[][] dir = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    public int maxAreaOfIsland(int[][] grid) {
        ans = 0;
        for(int i = 0;i < grid.length;i++){
            for(int j = 0;j < grid[0].length;j++){
                if(grid[i][j] == 1){
                    cur = 0;
                    dfs(i, j, grid);
                    ans = Math.max(ans, cur);
                }
            }
        }
        return ans;
    }

    public void dfs(int i, int j, int[][] grid){
        if(i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == 0){
            return;
        }
        cur++;
        grid[i][j] = 0;
        for(int[] d : dir){
            dfs(i + d[0], j + d[1], grid);
        }
    }
```
- **第k个排列**  
```
    int[] fac;
    int n;
    int k;
    boolean[] used;
    List<Integer> path;
    public String getPermutation(int n, int k) {
        this.n = n;
        this.k = k;
        used = new boolean[n+1];
        Arrays.fill(used, false);
        fac = new int[n+1];
        fac[0] = 1;
        for(int i = 1;i <= n;i++){
            fac[i] = fac[i-1] * i;
        }
        path = new ArrayList<>();
        dfs(0);
        StringBuilder stringBuilder = new StringBuilder();
        for (Integer c : path) {
            stringBuilder.append(c);
        }
        return stringBuilder.toString();
    }
    
    public void dfs(int id){
        if(id == n){
            return;
        }
        int cnt = fac[n-id-1];
        for(int i = 1;i <= n;i++){
            if(used[i]){
                continue;
            }
            if(cnt < k){
                k -= cnt;
                continue;
            }
            path.add(i);
            used[i] = true;
            dfs(id+1);
        }
    }
```
- **二叉树的所有路径**  
```
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        dfs(root, new String(), ans);
        return ans;
    }

    public void dfs(TreeNode node, String path, List<String> ans){
        if(node != null){
            StringBuilder sb = new StringBuilder(path);
            sb.append(node.val);
            if(node.left == null && node.right == null){
                ans.add(sb.toString());
            }else{
                sb.append("->");
                dfs(node.left, sb.toString(), ans);
                dfs(node.right, sb.toString(), ans);
            }
        }
    }
```
- **LRU缓存机制**  
```
class LRUCache {
    
    class DLinkedNode{
        int k;
        int v;
        DLinkedNode pre;
        DLinkedNode next;
        public DLinkedNode(){
        }
        public DLinkedNode(int k, int v){
            this.k = k;
            this.v = v;
        }
    }
    
    DLinkedNode head, tail;
    HashMap<Integer, DLinkedNode> cache;
    int capacity;
    int size;

    public LRUCache(int capacity) {
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.pre = head;
        this.capacity = capacity;
        size = 0;
        cache = new HashMap<>();
    }
    
    public int get(int key) {
        DLinkedNode t = cache.get(key);
        if(t == null){
            return -1;
        }
        moveToHead(t);
        return t.v;
    }
    
    public void put(int key, int value) {
        DLinkedNode t = cache.get(key);
        if(t != null){
            t.v = value;
            moveToHead(t);
        }else{
            DLinkedNode tt = new DLinkedNode(key, value);
            addToHead(tt);
            cache.put(key, tt);
            size++;
            if(size > capacity){
                cache.remove(tail.pre.k);
                remove(tail.pre);
                size--;
            }
        }
    }
    
    public void moveToHead(DLinkedNode node){
        remove(node);
        addToHead(node);
    }
    
    public void remove(DLinkedNode node){
        node.pre.next = node.next;
        node.next.pre = node.pre;
    } 
    
    public void addToHead(DLinkedNode node){
        node.next = head.next;
        head.next.pre = node;
        head.next = node;
        node.pre = head;
    }
}
```
- **外观数列**  
```
    public static String countAndSay(int n) {
        if(n == 1){
            return "1";
        }
        String pre = countAndSay(n-1);
        StringBuilder sb = new StringBuilder();
        int c = 1;
        for(int i = 0;i < pre.length()-1;i++){
            if(pre.charAt(i) == pre.charAt(i+1)){
                c++;
            }else{
                sb.append("" + c + pre.charAt(i));
                c = 1;
            }
        }
        sb.append("" + c + pre.charAt(pre.length()-1));
        return sb.toString();
    }
```