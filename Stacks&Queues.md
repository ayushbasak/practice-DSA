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