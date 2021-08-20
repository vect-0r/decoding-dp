


## Coin Change

Following are two versions of coin change problem: 
- The first one focuses on permutations of the coins. 
- The second one focuses on combinations of the coins. 

### Coin Change (LeetCode 322)

**Problem Link: https://leetcode.com/problems/coin-change/**

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

