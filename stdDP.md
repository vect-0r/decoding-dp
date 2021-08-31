
# Group 1

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

## Unbounded Knapsack - GFG

**Problem Link : https://practice.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1#**

```cpp

class Solution{
public:
    int knapSack(int N, int W, int val[], int wt[])
    {
        int dp[W + 1];
        
        memset(dp, 0, sizeof(dp));
        
        for (int i = 0; i <= W; ++i)
            for (int j = 0; j < N; j++)
                if (i >= wt[j])
                    dp[i] = max(dp[i], val[j] + dp[i - wt[j]]);
        
        return dp[W];
    }
};

```

- Observation: 
    - Duplicates allowed : 1D memo
    - Duplicates not allowed : 2D memo
    - One dp problem usually has many variations. You just have to solve a lot of problems to be able to identify the pattern.

# Group 2

## Count the number of binary strings of length N with no consecutive 1's allowed

**Problem Link : https://practice.geeksforgeeks.org/problems/consecutive-1s-not-allowed1912/1#**

```cpp

const ll MOD = 1e9 + 7;

class Solution{
public:
	// #define ll long long
	ll countStrings(int n) {
	    ll dp0[n + 1], dp1[n + 1];
	    memset(dp0, 0, sizeof(dp0));
	    memset(dp1, 0, sizeof(dp1));
	    dp0[1] = dp1[1] = 1;
	    for (ll i = 2; i <= n; ++i) {
	        dp0[i] = (dp0[i - 1] + dp1[i - 1]) % MOD;
	        dp1[i] = dp0[i - 1] % MOD;
	    }
	    return (dp0[n] + dp1[n]) % MOD;
	}
};

```

## Decode Ways - LC 91

**Problem Link : https://leetcode.com/problems/decode-ways/**

```cpp
class Solution {
public:
    int numDecodings(string s) {
        if (s.at(0) == '0')
            return 0;
        int n = s.size();
        int dp[n];
        dp[0] = 1;
        for (int i = 1; i < s.size(); ++i) {
            if (s.at(i - 1) == '0' && s.at(i) == '0') { // 21100 -> 211-00 , 2110-0
                dp[i] = 0;
            } else if (s.at(i - 1) == '0' && s.at(i) != '0') { // 21103 -> 211-03 , 2110-3 
                dp[i] = dp[i - 1];
            } else if (s.at(i - 1) != '0' && s.at(i) == '0') { 
                if (s.at(i - 1) == '1' || s.at(i - 1) == '2') { // 21120 -> 211-20 , 2112-0
                    dp[i] = (i >= 2 ? dp[i - 2] : 1);
                } else { // 21150 -> 211-50 , 2115-0
                    dp[i] = 0; 
                }
            } else { // 21123 -> 211-23 , 2112-3; 21176 -> 2117-6 , 211-76
                int lastTwo = stoi(s.substr(i - 1, 2));
                if (lastTwo <= 26) {
                    dp[i] = dp[i - 1] + (i >= 2 ? dp[i - 2] : 1);
                }
                else {
                    dp[i] = dp[i - 1];
                }
            }
        }
        return dp[n - 1];
    }
};
```


## Count Subsequences of the form a^ib^jc^k - GFG

**Problem Link : https://practice.geeksforgeeks.org/problems/count-subsequences-of-type-ai-bj-ck4425/1#**

```cpp
const int MOD = 1e9 + 7;

class Solution{
  public:
    // s : given string
    // return the expected answer
    int fun(string &s) {
        int a = 0;
        int ab = 0;
        int abc = 0;
        
        for (int i = 0; i < s.size(); ++i) {
            if (s.at(i) == 'a') {
                a = 1 + (2 * a) % MOD;
                a %= MOD;
            }
            else if (s.at(i) == 'b') {
                ab = a + (2 * ab) % MOD;
                ab %= MOD;
            }
            else if (s.at(i) == 'c') {
                abc = ab + (2 * abc) % MOD;
                abc %= MOD;
            }
        }
        
        return abc % MOD;
    }
};
```

## Stikler Thief - Find the Maximum Sum such that no two elements are Adjacent (Greedy + DP) (GFG)

**Problem Link : https://practice.geeksforgeeks.org/problems/stickler-theif-1587115621/1#**

- Observation : Note the use of Greedy approach with dp to eliminate some portion of recursion tree.

```cpp
class Solution
{
    public:
    //Function to find the maximum money the thief can get.
    int FindMaxSum(int arr[], int n)
    {
        int inc = arr[0];
        int exc = 0;
        for (int i = 1; i < n; ++i) {
            int n_inc = exc + arr[i];
            int n_exc = max(exc, inc);
            
            inc = n_inc;
            exc = n_exc;
        }
        
        int ans = max(inc, exc);
        return ans;
    }
};
```

## Friends Pairing Problem - GFG

**Problem Link : https://practice.geeksforgeeks.org/problems/friends-pairing-problem5425/1#**

```cpp
const int MOD = 1e9 + 7;

class Solution {
public:
    int countFriendsPairings(int n) {
        long long int dp[n + 1];
        
        memset(dp, 0, sizeof dp);
        
        dp[1] = 1;
        dp[2] = 2;
        
        for (int i = 3; i <= n; ++i) {
            dp[i] = dp[i - 1] % MOD; 
            dp[i] += (dp[i - 2] % MOD) * ((i - 1) % MOD) % MOD;
        }
        
        return dp[n] % MOD;
    }
};  
```

## Best Time to Buy and Sell Stock with Transaction Fee - LC 714

**Problem Link : https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/**

- Note : The discuss section contains a marvellous post by @fun4LeetCode
  - **https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/discuss/108870/Most-consistent-ways-of-dealing-with-the-series-of-stock-problems**

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int hold = -prices[0], cash = 0;
        for (int i = 1; i < prices.size(); ++i) {
            cash = max(cash, hold + prices[i] - fee);
            hold = max(hold, cash - prices[i]);
        }
        return cash;
    }
};
```

## Best Time to Buy and Sell Stock with Cooldown - LC 309

**Problem Link : https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/**

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int obsp = -prices[0], ocsp = 0, ossp = 0;
        for (int i = 1; i < prices.size(); ++i) {
            int nbsp = 0;
            int ncsp = 0;
            int nssp = 0;
            
            // cooldown + buy vs. buy
            if (ocsp - prices[i] > obsp) {
                nbsp = ocsp - prices[i];
            } else {
                nbsp = obsp;
            }
            
            // buy + sell vs. sell
            if (obsp + prices[i] > ossp) {
                nssp = obsp + prices[i];
            } else {
                nssp = obsp;
            }
            
            // cooldown vs. sell & cooldown
            if (ossp > ocsp) {
                ncsp = ossp;
            } else {
                ncsp = ocsp;
            }
            
            obsp = nbsp;
            ocsp = ncsp;
            ossp = nssp;
        }
        return max(ossp, ocsp);
    }
};
```

## Best Time to Buy and Sell Stock III - LC 123 (HARD)

**Problem Link : https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/**

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        
        int mpist = 0;
        int lsf = prices[0];
        int dpl[n];
        memset(dpl, 0, sizeof dpl);
        
        for (int i = 1; i < n; ++i) {
            if (prices[i] < lsf) {
                lsf = prices[i];
            }
            
            mpist = prices[i] - lsf;
            if (mpist > dpl[i - 1]) {
                dpl[i] = mpist;
            } else {
                dpl[i] = dpl[i - 1];
            }
        }
        
        int mpibt = 0;
        int msf = prices[n - 1];
        int dpr[n];
        memset(dpr, 0, sizeof dpr);
        
        for (int i = n - 2; i >= 0; --i) {
            if (prices[i] > msf) {
                msf = prices[i];
            }
            
            mpibt = msf - prices[i];
            if (mpibt > dpr[i + 1]) {
                dpr[i] = mpibt;
            } else {
                dpr[i] = dpr[i + 1];
            }
        }
        
        int op = 0;
        for (int i = 0; i < n; ++i) {
            op = max(op, dpl[i] + dpr[i]);
        }
        
        return op;
    }
};
```

## Best Time to Buy and Sell Stock IV - LC 188

- K Transactions allowed

**Problem Link : https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/**

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        
        if (n == 0) return 0;
        
        int dp[k + 1][n];
        memset(dp, 0, sizeof dp);
        
        for (int t = 1; t <= k; ++t) {
            int max = INT_MIN;
            
            for (int d = 1; d < n; ++d) {
                if (dp[t - 1][d - 1] - prices[d - 1] > max) {
                    max = dp[t - 1][d - 1] - prices[d - 1];
                }
                
                if (max + prices[d] > dp[t][d - 1]) {
                    dp[t][d] = max + prices[d];
                } else {
                    dp[t][d] = dp[t][d - 1];
                }
            }
        }
        
        return dp[k][n - 1];
    }
};
```
