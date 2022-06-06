# Backtracking

### Maximum Length of a Concatenated String with Unique Characters [leetcode](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/)

```cpp
bool unique(string x){
	int freq[26] = {0};
	for(char c: x){
		freq[c - 'a']++;
		if(freq[c-'a'] > 1)
			return false;
	}
	return true;
}
int res = 0;
void helper(vector<string> &arr, int pos, string curr){
	if(pos >= arr.size() or !unique(curr))
		return;
	res = res >= curr.length() ? res : curr.length();
	for(int i =pos; i< arr.size(); i++){
		helper(arr, i, curr + arr[i]);
	}
}
int maxLength(vector<string>& arr) {
	string temp = "";
	helper(arr, 0, temp);
	return res;
}
```

### Subsets [leetcode](https://leetcode.com/problems/subsets/)
```cpp
void helper(vector<int> &nums, int pos, vector<int> temp, vector<vector<int>> &res){
	res.push_back(temp);
	
	for(int i = pos; i < nums.size(); i++){
		temp.push_back(nums[i]);
		helper(nums, i+1, temp, res);
		temp.pop_back();
	}
}
vector<vector<int>> subsets(vector<int> &nums){
	vector<vector<int>> res;
	vector<int> temp;
	
	helper(nums, 0, temp, res);
	return res;
}
```

### Subsets II [leetcode](https://leetcode.com/problems/subsets-ii/submissions/)
```cpp
void helper(vector<int> &nums, vector<vector<int>> &res, vector<int> &temp, int n, int pos){
	res.push_back(temp);
	for(int i = pos; i < n; i++){
		if(i > pos and nums[i] == nums[i - 1]) continue;
		temp.push_back(nums[i]);
		helper(nums, res, temp, n, i + 1);
		temp.pop_back();
	}
}
vector<vector<int>> subsetsWithDup(vector<int>& nums) {
	vector<vector<int>> res;
	vector<int> temp;

	int n = nums.size();
	sort(nums.begin(), nums.end());
	helper(nums, res, temp, n, 0);

	return res;
}
```

### Permutations [leetcode](https://leetcode.com/problems/permutations/)
```cpp
bool contains(vector<int> A, int x){
	for(int a: A)
		if(a == x ) return true;
	return false;
}
void helper(vector<int> &nums, vector<int> temp, vector<vector<int>> &res){
	if(temp.size() == nums.size()){
		res.push_back(temp);
		return;
	}

	for(int i = 0; i< nums.size(); i++){
		if(contains(temp, nums[i])) continue;
		temp.push_back(nums[i]);
		helper(nums, temp, res);
		temp.pop_back();
	}
}
vector<vector<int>> permute(vector<int>& nums) {
	vector<vector<int>> res;
	vector<int> temp;
	helper(nums, temp, res);
	return res;
}
```

### Combination Sum [leetcode](https://leetcode.com/problems/combination-sum/)
```cpp
void helper(vector<int> &nums, int pos, int target, vector<int> temp, vector<vector<int>> &res){
	if(target < 0) return;
	if(!target){
		res.push_back(temp);
		return;   
	}

	for(int i = pos ; i < nums.size(); i++){
		if(target >= nums[i]){
			temp.push_back(nums[i]);
			helper(nums, i, target - nums[i], temp, res);
			temp.pop_back();
		}
	}
}
vector<vector<int>> combinationSum(vector<int> &nums, int target){
	vector<vector<int>> res;
	vector<int> temp;
	sort(nums.begin(), nums.end());
	helper(nums, 0, target, temp, res);
	return res;
}
```

### Combination Sum II [leetcode](https://leetcode.com/problems/combination-sum-ii/)
```cpp
void helper(vector<int> &candidates, vector<vector<int>> &res, vector<int> &temp, int n, int pos, int target){
	if(target < 0) return;
	if(!target){
		res.push_back(temp);
		return;
	}

	for(int i = pos; i < n; i++){
		if(i > pos and candidates[i] == candidates[i - 1]) continue;
		temp.push_back(candidates[i]);
		helper(candidates, res, temp, n, i + 1, target - candidates[i]);
		temp.pop_back();
	}
}
vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
	vector<vector<int>> res;
	vector<int> temp;

	int n = candidates.size();
	sort(candidates.begin(), candidates.end());
	helper(candidates, res, temp, n, 0, target);

	return res;
}
```

### Letter Combinations of a Phone number [leetcode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
```cpp
vector<string> mp = {
	"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"
};
void helper(string digits, int pos, string curr, vector<string> &res){
	if(pos == digits.length()){
		res.push_back(curr);
		return;
	}

	for(char c: mp[digits[pos] - '0']){
		helper(digits, pos + 1, curr + c, res);
	}
}
vector<string> letterCombinations(string digits) {
	if(digits == "")
		return {};
	vector<string> res;
	string temp = "";
	helper(digits, 0, temp, res);
	return res;
}
```

### N Queens [leetcode](https://leetcode.com/problems/n-queens/)
```cpp
bool isValid(vector<string> &board, int row, int col, int n) {
	for (int i = 0; i < row; i++)
		if (board[i][col] == 'Q')
			return false;
	for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
		if (board[i][j] == 'Q')
			return false;
	for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++)
		if (board[i][j] == 'Q')
			return false;
	return true;
}

void helper(vector<vector<string>> &res, vector<string> curr, int n, int row){
	if (row == n) {
		res.push_back(curr);
		return;
	}
	for (int j = 0; j < n; j++)
		if (isValid(curr, row, j, n)) {
			curr[row][j] = 'Q';
			helper(res, curr, n, row + 1);
			curr[row][j] = '.';
		}
}
vector<vector<string>> solveNQueens(int n) {
	vector<vector<string>> res;
	vector<string> temp(n, string(n, '.'));
	helper(res, temp, n, 0);
	return res;
}
```