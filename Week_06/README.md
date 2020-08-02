学习笔记


# 学习笔记



# 动态规划关键点

1. 最优子结构  opt[n] = best_of(opt[n-1], opt[n-2], ....)

2. 储存中间状态： opt[i]

3. 递推公式 （美名曰： 状态转移方程或者DP`方程）

   Fib: opt[i] = opt[i - 1] + opt[i - 2] 

状态转移方程 （DP方程）

![image-20200727212341131](/home/hit/.config/Typora/typora-user-images/image-20200727212341131.png)

# 题目 最小路径和

给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

示例:

输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-path-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int route = 0;
        if (grid.size() == 0) return 0;
        vector<vector<int>> opt(grid.size(), vector<int>(grid[0].size()));
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[0].size(); j++) {
                if (i == 0 || j == 0) {
                    if (i == 0 && j == 0) opt[i][j] = grid[i][j];
                    else if (j > 0) opt[i][j] = grid[i][j] + opt[i][j - 1];
                    else opt[i][j] = grid[i][j] + opt[i - 1][j];
                }
                else opt[i][j] = min({opt[i - 1][j], opt[i][j - 1]}) + grid[i][j];
            }
        }
        return opt[grid.size() - 1][grid[0].size() - 1];
    }
};
```

# 题目 不同路径

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

例如，上图是一个7 x 3 的网格。有多少可能的路径？

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/robot_maze.png) 

示例 1:

输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。

1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右

示例 2:

输入: m = 7, n = 3
输出: 28

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-paths
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> res(m, vector<int>(n));

        for (int i = 0; i < res.size(); i++) {
            for (int j = 0; j < res[0].size(); j++) {
                if (i == 0 || j == 0) 
                    res[i][j] = 1;
                else res[i][j] = res[i - 1][j] + res[i][j - 1];
            }
        }
        return res[m - 1][n - 1];
    }
};
```



# 零钱兑换专题

## 题目 零钱兑换（https://leetcode-cn.com/problems/coin-change）

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

```
示例 1:

输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1

示例 2:

输入: coins = [2], amount = 3
输出: -1 

说明:
你可以认为每种硬币的数量是无限的。

```

### 解决思路：

### 1.自底向上动态递推

F(S)：组成金额 S 所需的最少硬币数量

[c0…cn−1] ：可选的 n枚硬币面额值

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if (coins.size() == 0) return -1;
        vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {   // i represent amount
            for (auto it : coins) {
                if (i >= it) dp[i] = min(dp[i - it] + 1, dp[i]);
            }
        }
        return amount < dp[amount]? -1 : dp[amount];
    }
};
```



