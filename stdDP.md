


## Coin Change

Following are two versions of coin change problem: 
- The first one focuses on permutations of the coins. 
- The second one focuses on combinations of the coins. 

### Coin Change (LeetCode 322)

**Problem Link : https://leetcode.com/problems/coin-change/**

```cpp

const int inf = 1e9;

class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, inf); // dp[i] : min coins to reach amount i
        
        dp[0] = 0;
        
        for (int i = 1; i <= amount; ++i)
            for (int j = 0; j < coins.size(); ++j)
                if (i >= coins[j])
                    dp[i] = min(1 + dp[i - coins[j]], dp[i]);
        
        return dp[amount] >= inf ? -1 : dp[amount];
    }
};

```

### Coin Change 2 (LeetCode 518)

**Problem Link : https://leetcode.com/problems/coin-change-2/**

```cpp

class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0); // dp[i] : total coins to reach amount i
        
        dp[0] = 1;
        
        for (int coin : coins) 
            for (int i = coin; i <= amount; ++i) 
                dp[i] += dp[i - coin];
        
        return dp[amount];
    }
};

```

## Subset Sum Problem - GFG

**Problem Link : https://practice.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1/** 

```cpp

class Solution{   
public:
    bool isSubsetSum(int n, int arr[], int sum){
        int dp[n + 1][sum + 1];
        
        memset(dp, 0, sizeof(dp));
        
        for (int i = 0; i < n + 1; ++i) {
            for (int j = 0; j <= sum; ++j) {
                if (i == 0 && j == 0) {
                    dp[i][j] = true;
                } else if (i == 0) {
                    dp[i][j] == false;
                } else if (j == 0) {
                    dp[i][j] = true;
                } else {
                    if (j < arr[i - 1]) {
                        dp[i][j] = dp[i - 1][j];
                    } else {
                        dp[i][j] = dp[i - 1][j] || dp[i - 1][j - arr[i - 1]];
                    }
                }
            }
        }
        
        return dp[n][sum];
    }
};

```

## Min Cost Climbing Stairs - LC 746

**Problem Link : https://leetcode.com/problems/min-cost-climbing-stairs/**

```cpp

class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        vector<int> dp(n + 1, 0); // dp[i] : cost to reach i from 0
        
        if (n == 1) return cost[0];
        
        dp[0] = cost[0];
        dp[1] = cost[1];
        
        for (int i = 2; i < n; ++i)
            dp[i] = cost[i] + min(dp[i - 1], dp[i - 2]);
        
        dp[n] = min(dp[n - 1], dp[n - 2]);
        
        return dp[n];
    }
};

```

## 0/1 Knapsack - GFG

**Problem Link : https://practice.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1#**

- Also has a 1-D dp space-optimised solution, instead of the 2-D dp solution below. 

```cpp

class Solution
{
    public:
    //Function to return max value that can be put in knapsack of capacity W.
    int knapSack(int W, int wt[], int val[], int n) 
    {
        int dp[n + 1][W + 1]; // dp[i][j] : max. val. if first i weights are used to fill 
                              // knapsack of capacity j
        
        memset(dp, 0, sizeof(dp));
        
        for (int i = 1; i < n + 1; ++i) {
            for (int j = 1; j < W + 1; ++j) {
                if (j >= wt[i - 1])
                    dp[i][j] = max(val[i - 1] + dp[i - 1][j - wt[i - 1]],
                                    dp[i - 1][j]);
                else
                    dp[i][j] = dp[i - 1][j];
            }
        }
        
        return dp[n][W];
    }
};

```
