<!--{
    "tags": ["动态规划", "DFS2DP"],
    "来源": "LeetCode",
    "难度": "中等",
    "编号": "0198",
    "标题": "打家劫舍",
    "公司": []
}-->

<summary><b>问题简述</b></summary>

```txt
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。
```
> [198. 打家劫舍 - 力扣（LeetCode）](https://leetcode-cn.com/problems/house-robber/)

<!-- 
<details><summary><b>详细描述</b></summary>

```txt
```
-->

</details>

<!-- <div align="center"><img src="../../../_assets/xxx.png" height="300" /></div> -->

<summary><b>思路</b></summary>

- 定义 `dfs(i)` 表示前 `i` 家能打劫的最大价值；
- 【递归基】`i <= 0` 时，有 `dfs(0) = 0`；
    > 小细节：因为会用到 ` i-2` 的状态，所以需要定义 `i < 0` 时的状态；
- 递推公式：`dfs(i) = max(dfs(i-1), dfs(i-2) + nums[i-1])`；
    > 对第 `i` 家（`nums[i-1]`），有两种可能，不抢（`dfs(i-1)`），抢（`dfs(i-2) + nums[i-1]`），去其中的较大值；

<details><summary><b>Python：递归</b></summary>

```python
class Solution:
    def rob(self, nums: List[int]) -> int:

        from functools import lru_cache

        @lru_cache(maxsize=None)
        def dfs(i):
            if i == 0:  # 显然
                return 0
            if i == 1:  # 只有一家时，必抢
                return nums[0]
            
            r1 = dfs(i - 1)  # 不抢
            r2 = dfs(i - 2) + nums[i - 1]  # 抢
            return max(r1, r2)
        
        N = len(nums)
        return dfs(N)
```

</details>


<details><summary><b>Python：动态规划</b></summary>

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        
        N = len(nums)
        dp = [0] * (N + 1)
        dp[1] = nums[0]

        for i in range(2, N + 1):
            r1 = dp[i - 1]  # 不抢
            r2 = dp[i - 2] + nums[i - 1]  # 抢
            dp[i] = max(r1, r2)

        return dp[-1]
```

</details>