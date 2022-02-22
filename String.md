# String
miscellaneous problems based on string

### Longest Palindrome without repeating characters [leetcode](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```cpp
int lengthOfLongestSubstring(string s) {
	unordered_map<int, int> mp;

	int max_length = 0;
	int l = 0, r = 0;
	while(l <=r and r < s.size()){
		if(!mp[s[r]]){
			max_length = max(max_length, r - l + 1);
		}
		else{
			while(l <= r and mp[s[r]]){
				mp[s[l]] = 0;
				l++;
			}
		}
		mp[s[r]] = 1;
		r++;
	}

	return max_length;
}
```
