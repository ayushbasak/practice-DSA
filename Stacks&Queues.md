# Stacks and Queues
Leetcode / InterviewBit Questions on Stacks and Queues

### Balanced Paranthesis [interviewbit](https://www.interviewbit.com/problems/balanced-parantheses/)
```cpp
bool isBalanced(string A){
	int curr = 0;
	for(char c: A){
		if(c == '(')
			curr++;
		else curr--;
		
		if(curr < 0) return false;
	}
	return !curr;
}
```

### Redundant Braces [interviewbit](https://www.interviewbit.com/problems/redundant-braces/)
```cpp
bool braces(string A) {
	stack<char> s;
	for(char c: A){
		if(c =='(' or c == '+' or c == '*' or c == '-' or c == '/')
			s.push(c);
		else if(c == ')'){
			if(s.top() == '(')
				return true;
			else{
				while(!s.empty() and s.top() != '(')
					s.pop();
				s.pop();
			}
		}
	}
	return false;
}
```

### Sliding Window Maximum [leetcode](https://leetcode.com/problems/sliding-window-maximum/)
```cpp
vector<int> slidingMax(vector<int> &nums, int k){
	deque<int> indexes;
	vector<int> res;
	
	for(int i =0; i < nums.size(); i++){
		while(!indexes.empty() and indexes.front() < i - k + 1)
			indexes.pop_front();
		while(!indexes.empty() and nums[indexes.back()] < nums[i])
			indexes.pop_back();
		indexes.push_back(i);
		if(i >= k-1)
			res.push_back( nums[indexes.front()] );
	}
	return res;
}
```

### Nearest Smallest Element [interviewbit](https://www.interviewbit.com/problems/nearest-smaller-element/)
```cpp
vector<int> nearestSmallest(vector<int> &A){
	vector<int> res;
	stack<int> s;
	for(int num: A){
		if(s.empty()){
			res.push_back(-1);
		}
		else{
			while(!s.empty() and s.top() >= num)
				s.pop();
			res.push_back(num);
		}
		s.push(num);
	}
	return res;
}
```

### Final Prices with Special Discount [leetcode](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop/)
```cpp
vector<int> finalPrices(vector<int>& prices) {
	stack<int> st;
	int n = prices.size();
	for(int i = 0; i < n; i++){
		while(!st.empty() and prices[st.top()] >= prices[i]){
			prices[st.top()] -= prices[i];
			st.pop();
		}
		st.push(i);
	}
	return prices;
}
```