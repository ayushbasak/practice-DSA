# Dynamic Programming
### Longest Increasing Subsequence [leetcode](https://leetcode.com/problems/longest-increasing-subsequence/)
1. Solution Using DP
>  O(N^2) Time O(N) Space

```cpp
int lengthOfLis(vector<int> &nums){
	int n = nums.size();
	vector<int> dp(n, 1);
	for(int i =1; i<n; i++){
		for(int j =0; j<i; j++){
			if(nums[j] < nums[i])
				dp[i] = max(dp[i], 1 + dp[j]);
		}
	}
	
	int max_element = 0;
	for(int i =0; i<n; i++)
		max_element = max(max_element, dp[i]);
	
	return max_element;
}
```

### Climbing Stairs [leetcode](https://leetcode.com/problems/climbing-stairs/)

```cpp
int climbStairs(int n){
	vector<int> dp(n, 1);
	if(n <= 2)
		return n;
	for(int i = n-3; i>=0; i--){
		dp[i] = dp[i+1] + dp[i+2];
	}
	
	return dp[0] + dp[1];
}
```

__Reverse Fibonacci__
```cpp
int climbStairs(int n){
	int a = 1;
	int b = 1;
	if(n <= 2)
		return n;
	for(int i = n-3; i>=0; i--){
		int temp = a + b;
		b = a;
		a = temp;
	}
	return a + b;
}
```

### Paint House [interviewbit](https://www.interviewbit.com/problems/paint-house/)

```cpp
int minCost(vector<vector<int>> &A){
	int n = A.size();
	int dp[n][3];
	
	dp[0][0] = A[0][0];
	dp[0][1] = A[0][1];
	dp[0][2] = A[0][2];
	for(int i =1; i<n; i++){
		for(int j=0; j<3; j++){
			dp[i][j] = A[i][j];
			if(j == 0)
				dp[i][j] += min(dp[i-1][1], dp[i-1][2]);
			else if(j == 1)
				dp[i][j] += min(dp[i-1][0], dp[i-1][2]);
			else
				dp[i][j] += min(dp[i-1][0], dp[i-1][1]);
		}
	}
	
	return min({dp[n-1][0], dp[n-1][1], dp[n-1][2]});
}
```

### Minimum Path Sum [leetcode](https://leetcode.com/problems/minimum-path-sum/)
```cpp
int minPathSum(vector<vector<int>>& grid) {
	int m = grid.size();
	if(!m)
		return 0;
	int n = grid[0].size();

	int dp[m][n];
	dp[0][0] = grid[0][0];

	for(int i = 1; i < m; i++)
		dp[i][0] = dp[i-1][0] + grid[i][0];

	for(int j = 1; j < n; j++)
		dp[0][j] = dp[0][j-1] + grid[0][j];

	for(int i = 1; i<m; i++){
		for(int j = 1; j< n; j++){
			dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
		}
	}
	return dp[m-1][n-1];
}
```

### Triangle [leetcode](https://leetcode.com/problems/triangle/)
```cpp
int minimumTotal(vector<vector<int>>& triangle) {
        int r = triangle.size();
        
        vector<int> res = triangle[r-1];
        
        for ( int i = r - 2; i >= 0 ; i-- )
            for ( int j = 0; j < triangle[i].size() ; j++ )
                res[j] = triangle[i][j] + min(res[j], res[j+1]);
        return res[0];
    }
```

### Maximal Square
[reference: Errichto](https://youtu.be/oPrpoVdRLtg)
```cpp
int maximalSquare(vector<vector<char>> &matrix){
	int m = matrix.size();
	if(!m) return 0;
	int n = matrix[0].size();
	
	int dp[301][301];
	int res = 0;
	
	for(int i =0; i<m; i++){
		for(int j =0; j<n; j++){
			if(matrix[i][j] == '1'){
				dp[i][j] = 1;
				if(i > 0 and j > 0){
					dp[i][j] += min({
						dp[i-1][j],
						dp[i][j-1],
						dp[i-1][j-1]
					});
				}
			}
			else
				dp[i][j] = matrix[i][j] - '0';
			res = max(res, dp[i][j]);
		}
	}
	// return area
	return res * res;
}
```

### Longest Common Subsequence [leetcode](https://leetcode.com/problems/longest-common-subsequence/)
__Recursive Memoized__

```cpp
int dp[1000][1000];
int helper(string A, string B, int a, int b){
	if(!a or !b) return 0;
	
	if(dp[a][b])
		return dp[a][b];
	
	if(A[a] == A[b])
		return dp[a][b] = helper(A, B, a-1, b-1) + 1;
	return dp[a][b] = max({
		helper(A, B, a - 1, b),
		helper(A, B, a, b-1)
	});
}
int lcs(string A, string B){
	return helper(A, B, A.length() -1, B.length() -1);
}
```

### Unique Paths [leetcode](https://leetcode.com/problems/unique-paths/)
```cpp
int uniquePaths(int m, int n) {
	int dp[100][100];

	for(int i =0; i<m; i++)
		dp[i][0] = 1;
	for(int j=0; j<n; j++)
		dp[0][j] = 1;

	for(int i =1; i<m; i++){
		for(int j =1; j<n; j++)
			dp[i][j] = dp[i-1][j] + dp[i][j-1];
	}
	return dp[m-1][n-1];
}
```

### Number of ways to paint 3 x N grid [leetcode](https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid/)

Math DP [reference](https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid/discuss/574923/JavaC%2B%2BPython-DP-O(1)-Space)

```cpp
int numOfWays(int n) {
	long xyz = 6, xyx = 6;
	long XYZ, XYX;

	long mod = 1e9 + 7;
	for(int i =1; i<n; i++){
		XYZ = 3 * xyz + 2 * xyx;
		XYX = XYZ - xyz;

		xyz = XYZ % mod;
		xyx = XYX % mod;
	}

	return (int)((xyz + xyx) % mod);
}
```

### Coin Change [leetcode](https://leetcode.com/problems/coin-change/)
__Recursive Memoized__
```cpp
unordered_map<int, int> mp;
int coinChange(vector<int>& coins, int amount) {
	if(amount < 0) return -1;
	else if(!amount) return 0;

	if(mp[amount]) return mp[amount];
	int best = INT_MAX;
	for(int coin: coins){
		int k = coinChange(coins, amount - coin);
		if(k >= 0 and k < best)
			best = k + 1;
	}

	return mp[amount] = best == INT_MAX ? -1: best;
}
```
__Iterative__
```cpp
int coinChange(vector<int>& coins, int amount) {
	vector<int> dp(amount + 1, amount + 1);
	dp[0] = 0;
	for (int i = 1; i <= amount; i++) {
		for (int j = 0; j < coins.size(); j++) {
			if (coins[j] <= i) {
				dp[i] = min(dp[i], dp[i - coins[j]] + 1);
			}
		}
	}
	return dp[amount] > amount ? -1 : dp[amount];
}
```