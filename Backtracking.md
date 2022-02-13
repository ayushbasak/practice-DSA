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

### Combination sum [leetcode](https://leetcode.com/problems/combination-sum/)
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