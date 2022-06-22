# Arrays
Leetcode / InterviewBit Questions based on Arrays and Miscellaneous

### 3Sum [leetcode]()
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

### Maximum Gap [leetcode]()
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