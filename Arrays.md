# Arrays
Leetcode / InterviewBit Questions based on Arrays and Miscellaneous

### 3Sum [leetcode](https://leetcode.com/problems/3sum/)
__Approach__:  
- for every element, perform __2Sum__. 
- To prevent duplicate elements, check for them with triplet

__Complexity__:  

| Time | Space |
| --- | --- |
| O(N<sup>2</sup>) | O(1) | 

```cpp
vector<vector<int>> threeSum(vector<int>& numbers) {
	sort(numbers.begin(), numbers.end());
	vector<vector<int>> res;
	int n = numbers.size();
	for (int i =0; i < n; i++) {
		int target = -numbers[i];
		int left = i + 1;
		int right = n - 1;
		while (left < right) {
			int sum = numbers[left] + numbers[right];
			if (sum < target)
				left++;
			else if (sum > target)
				right--;
			else {
				vector<int> triplet = { numbers[i], numbers[left], numbers[right] };
				res.push_back(triplet);
				while(left < right and numbers[left] == triplet[1])
					left++;
				while(left < right and numbers[right] == triplet[2])
					right--;
			}
		}
		while (i + 1 < n and numbers[i+1] == numbers[i])
			i++;
	}
	return res;
}
```

### Maximum Gap [leetcode](https://leetcode.com/problems/maximum-gap/)
[reference](https://leetcode.com/problems/maximum-gap/discuss/50643/bucket-sort-JAVA-solution-with-explanation-O(N)-time-and-space)

__Approach__:  
- __Bucket Sort__
- Find the largest and smallest element
- Create buckets of size N - 1 and try to fill the remaining N - 2 element in these buckets
- By piegonhole principle, there will be at least one bucket with no element
- Store the maximum and minimum element in a bucket
- The maximum gap occurs between two buckets, min element of bucket[i] - max element fo bucket[i - 1]

__Complexity__:  

| Time | Space |
| --- | --- |
| O(N) | O(N) |

```cpp
int maximumGap(vector<int> &nums) {
	int n = nums.size();
	if (n < 2)
		return 0;

	int max_element = 0;
	int min_element = INT_MAX;

	for (int num: nums) {
		max_element = max(max_element, num);
		min_element = min(min_element, num);
	}

	int bucketSize = (int) ceil( (double) (max_element - min_element) / (n - 1) );
	vector<int> bucketsMax(n - 1, INT_MIN);
	vector<int> bucketsMin(n - 1, INT_MAX);

	for (int num: nums) {
		if (num == min_element or num == max_element)
			continue;

		int index = (num - min_element) / bucketSize;
		bucketsMax[index] = max(num, bucketsMax[index]);
		bucketsMin[index] = min(num, bucketsMin[index]);
	}

	int max_gap = INT_MIN;
	int prev = min_element;

	for (int i =0; i < n - 1; i++) {
		if (bucketsMin[i] == INT_MAX and bucketsMax[i] == INT_MIN)
			continue;

		max_gap = max(max_gap, bucketsMin[i] - prev);
		prev = bucketsMax[i];
	}
	max_gap = max(max_gap, max_element - prev);
	return max_gap;
}
```

### Majority Element [leetcode](https://leetcode.com/problems/majority-element/)
__Approach__:  
- Boyer Moore Majority Vote Algorithm
- It is gauranteed that there is at least one element whose count is greater than __ceil (n / 2)__
- If we make pairs of non similar elements, all elements exhaust themselves, and only the element in majority remains
__Complexity__:  

| Time | Space |
| --- | --- |
| O(N) | O(1) |

```cpp
int majorityElement(vector<int>& nums) {
	int maj = nums[0], count = 1;
	for(int i =1;i< nums.size();i++){
		if(!count){
			count++;
			maj = nums[i];
		}
		else if(maj == nums[i])
			++count;
		else
			--count;
	}
	return maj;
}
```

### Majority Element II [leetcode](https://leetcode.com/problems/majority-element-ii/)
[reference](https://gregable.com/2013/10/majority-vote-algorithm-find-majority.html)
__Approach__:  
- Boyer Moore Majority Voting Algorithm
- There can be either 1 or 2 majority elements with __count > ceil ( n / 3)__.
- Create pairs of these assumed two elements with non majority elements, till we find one or two majority.

__Complexity__:  

| Time | Space |
| --- | --- |
| O(N) | O(1) |
```cpp
vector<int> majorityNumber(vector<int> &nums) {
	int num1 = nums[0], num2 = nums[0];
	int count1 = 0, count2 = 0;
	for (int num: nums) {
		if (num == num1)
			++count1;
		else if (num == num2)
			++count2;
		else if (!count1) {
			count1 = 1;
			num1 = num;
		}
		else if (!count2) {
			count2 = 1;
			num2 = num;
		}
		else {
			count1--;
			count2--;
		}
	}
	count1 = 0; count2 = 0;
	for (int num: nums) {
		if (num == num1) ++count1;
		else if (num == num2) ++count2;
	}

	vector<int> res;
	if (count1 > nums.size() / 3) res.push_back(num1);
	if (count2 > nums.size() / 3) res.push_back(num2);

	return res;
}
```

### Largest Number [leetcode](https://leetcode.com/problems/largest-number/)
__Approach__:  
- Sort the list according to which combination of two strings is larger
- edge case, if sorted list contains even a single 0 in the beginning, return "0"
__Complexity__: 

| Time | Space |
| --- | --- |
| O(k N logN ) | O(N) |

```cpp
static bool comp(string &s1, string &s2) {
	return s1 + s2 > s2 + s1;
}
string largestNumber(vector<int>& nums) {
	vector<string> temp;

	for (int num: nums) {
		temp.push_back(to_string(num));
	}

	sort(temp.begin(), temp.end(), comp);
	string res = "";

	if (temp[0][0] == '0')
		return "0";

	for (string num: temp) {
		res += num;
	}

	return res;
}
```

### Jump Game [leetcode](https://leetcode.com/problems/jump-game/)

```cpp
bool canJump(vector<int>& nums) {
	int n = nums.size();
	int last = n - 1;
	for (int i = n - 2; i >= 0; i--) {
		if (i + nums[i] >= last)
			last = i;
	}
	
	return last <= 0;
}
```

### Jump Game II [leetcode](https://leetcode.com/problems/jump-game-ii/)

```cpp
int jump(vector<int>& nums) {
	int jumps = 0;
	int end = 0, far = 0;
	for (int i = 0; i < nums.size() - 1; i++) {
		far = max(far, i + nums[i]);
		if (i == end) {
			jumps++;
			end = far;
		}
	}
	
	return jumps;
}
```

### Jump Game III [leetcode](https://leetcode.com/problems/jump-game-iii/)

```cpp
bool canReach(vector<int>& arr, int start) {
	if (start < 0 or start >= arr.size() or arr[start] < 0)
		return false;
	
	arr[start] *= -1;
	
	return arr[start] == 0 or 
		canReach(arr, start - arr[start]) or
		canReach(arr, start + arr[start]);
}
```

### Jump Game IV [leetcode](https://leetcode.com/problems/jump-game-iv/)

```cpp
int minJumps(vector<int>& arr) {
	int n = arr.size();
	unordered_map<int, vector<int>> mp;
	for (int i = 0; i < n; i++) {
		mp[arr[i]].push_back(i);    
	}
	vector<bool> visited(n); 
	visited[0] = true;
	queue<int> q; 
	q.push(0);
	int count = 0;
	while (!q.empty()) {
		for (int size = q.size(); size > 0; size--) {
			int index = q.front(); q.pop();
			if (index == n - 1) 
				return count;
			vector<int>& next = mp[arr[index]];
			next.push_back(index - 1); 
			next.push_back(index + 1);
			for (int j : next) {
				if (j >= 0 and j < n and !visited[j]) {
					visited[j] = true;
					q.push(j);
				}
			}
			next.clear();
		}
		count++;
	}
	return 0;
}
```