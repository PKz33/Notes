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
1. 解题思路：`BFS`填海造陆，题意有点绕，求每一个海洋区域的最近陆地距离，然后返回这些距离的最大值  
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
