# String
miscellaneous problems based on string

### Longest Palindrome without repeating characters [leetcode](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

__Approach__:  
- Sliding Window
- move outward keeping a track of which character has been seen before
- move left pointer forward, until seen character is removed from substring
- keep maximizing substring length

__Complexity__:  

| Time | Space |
| --- | --- |
| O(<sup>2</sup>) | O(N) |

_The inner while loop does not run for N times_

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

### Longest Palindromic Substring [leetcode](https://leetcode.com/problems/longest-palindromic-substring/)
__Approach__:  
- for every character in string, expand outward to check if that substrings are palindromes
- for even lengths expand from two adjacent characters outward

__Complexity__:  

| Time | Space |
| --- | --- |
| O(N<sup>2</sup>) | O(1) | 

_worst case if string is made up of same characters, and every character needs to expand to full string length_  
 
```cpp
string longestPalindrome(string s) {
	string max_string = "";
	int max_length = 0;

	int n = s.size();
	for (int i =0; i < n; i++) {
		int l, r;

		l = i, r = i;
		while (l >= 0 and r < n and s[l] == s[r]) {
			if (r - l + 1 > max_length) {
				max_length = r - l + 1;
				max_string = s.substr(l, max_length);
			}
			--l; r++;
		}

		l = i; r = i + 1;
		while (l >= 0 and r < n  and s[l] == s[r]) {
			if (r - l + 1 > max_length) {
				max_length = r - l + 1;
				max_string = s.substr(l, max_length);
			}
			--l; r++;
		}
	}

	return max_string;
}
```