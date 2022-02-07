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