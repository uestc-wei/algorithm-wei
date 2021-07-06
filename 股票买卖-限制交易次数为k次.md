<[动态规划（「力扣」更新过用例，只有优化空间的版本可以 AC） - 买卖股票的最佳时机 IV - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/solution/dong-tai-gui-hua-by-liweiwei1419-4/)>

```java
思想：
    首先因为交易一次需要两天，买入和卖出。所以当k>=len/2时，变成了不限制交易次数。
    直接用贪心。
    
    状态方程：
	dp[i][j][1] 第i天，第j次交易，0：不持股，1：持股
    dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i - 1]);
    dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i - 1]);
    优化空间：
    dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - price);
    dp[j][0] = Math.max(dp[j][0], dp[j][1] + price);
public class Solution {

    public int maxProfit(int k, int[] prices) {
        int len = prices.length;
        if (k == 0 || len < 2) {
            return 0;
        }
        if (k >= len / 2) {
            return greedy(prices);
        }

        int[][] dp = new int[k + 1][2];
        for (int i = 0; i <= k; i++) {
            dp[i][1] = Integer.MIN_VALUE;
        }
        for (int price : prices) {
            for (int j = 1; j <= k; j++) {
                dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - price);
                dp[j][0] = Math.max(dp[j][0], dp[j][1] + price);
            }
        }
        return dp[k][0];
    }

    private int greedy(int[] prices) {
        int res = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) {
                res += prices[i] - prices[i - 1];
            }
        }
        return res;
    }
}
O(NK)
```

