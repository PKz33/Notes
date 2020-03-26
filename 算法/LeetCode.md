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
