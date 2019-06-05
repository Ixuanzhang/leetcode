1、中文题目链接：<https://leetcode-cn.com/problems/length-of-longest-fibonacci-subsequence/>

2、题目

​	如果序列 `X_1, X_2, ..., X_n` 满足下列条件，就说它是 *斐波那契式* 的：

- `n >= 3`

- 对于所有 `i + 2 <= n`，都有 `X_i + X_{i+1} = X_{i+2}`

  给定一个**严格递增**的正整数数组形成序列，找到 `A` 中最长的斐波那契式的子序列的长度。如果一个不存在，返回  0 。

  *（回想一下，子序列是从原序列 A 中派生出来的，它从 A 中删掉任意数量的元素（也可以不删），而不改变其余元素的顺序。例如， [3, 5, 8] 是 [3, 4, 5, 6, 7, 8] 的一个子序列）*

  **示例 1：**

  ```
  输入: [1,2,3,4,5,6,7,8]
  输出: 5
  解释:
  最长的斐波那契式子序列为：[1,2,3,5,8] 。
  ```

  **示例 2：**

  ```
  输入: [1,3,7,11,12,14,18]
  输出: 3
  解释:
  最长的斐波那契式子序列有：
  [1,11,12]，[3,11,14] 以及 [7,11,18] 。
  ```

  **提示：**

  - `3 <= A.length <= 1000`
  - `1 <= A[0] < A[1] < ... < A[A.length - 1] <= 10^9`
  - *（对于以 Java，C，C++，以及 C# 的提交，时间限制被减少了 50%）*

3、解题思路：

		采取动态规划，而不是dfs搜索。

​	我们假设dp[i][j]是以A【i】与A【j】结尾的数列（因为默认了Ai<Aj）。

​	所以对于类似......A[i],A[j]这个数列如果有A[k]==A[i]+A[j]，那么这数列就变成了.....A[i],A[j],A[k].

​	dp[j][k]=dp[i][j]+1

​	上述式子便是转移方程。

​	我们考虑初始化的情况：对于dp[i][i]意思是两个相同的数字做结尾，是不存在的！因此dp[i][i]=0

​	而其他的情况，只要处于两个不同的式子，那么长度就是2。所以 dp[i][j] =2.

4、代码

class Solution {
public:
    int lenLongestFibSubseq(vector<int>& A) {
        int len = A.size();
        unordered_map<int,int> dict;
        for(int i=0;i<len;i++){
            dict[A[i]] = i;
        }
        int dp[len][len];
        memset(dp,0,sizeof(dp));
        int ans = 0;
        for(int j=1;j<len;j++)
            dp[0][j]=2;
        for(int i=1;i<len;i++){
            for(int j=i+1;j<len;j++){
                int cha = A[j] - A[i];
                if(dict.find(cha)!=dict.end() && dict[cha]<i){
                        dp[i][j]=dp[dict[cha]][i]+1;
                }
                else
                    dp[i][j]=2;
                if(dp[i][j] > ans){
                    ans = dp[i][j];
                }
            }    
        }
        if(ans==2)
            return 0;
        return ans;
    }
};