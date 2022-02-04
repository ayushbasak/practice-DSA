# Hashing

### Anagrams [interviewbit](https://www.interviewbit.com/problems/anagrams/)
```cpp
vector<vector<int>> anagrams(vector<string> &s){
	vector<vector<int>> res;
	unordered_map<string, vector<int>> mp;
	
	for(int i =0; i<s.size(); i++){
		string temp = s[i];
		sort(temp.begin(), temp.end());
		
		if(mp[temp]){
			mp[temp].push_back(i+1);
		}
		else{
			vector<int> indices;
			indices.push_back(i+1);
			mp[temp] = indices;
		}
	}
	
	for(auto x: m)
		res.push_back(x);
	return res;
}
```

### Random Pointers Copy List [leetcode](https://leetcode.com/problems/copy-list-with-random-pointer/)

**Solution using Hash Map** 
> O(N) Time O(N) Space

```cpp
Node* copyRandomList(Node* head) {
	Node * ptr = head;
	unordered_map<Node *, Node *> mp;
	while(ptr){
		Node * newNode = new Node(ptr->val);
		mp[ptr] = newNode;
		ptr = ptr->next;
	}

	ptr = head;
	while(ptr){
		mp[ptr]->next = mp[ptr->next];
		mp[ptr]->random = mp[ptr->random];
		ptr = ptr->next;
	}
	return mp[head];
}
```

**Solution using Linked Lists**
> O(N) Time O(1) Space
[reference](https://www.youtube.com/watch?v=OvpKeraoxW0)

```cpp
Node* copyRandomList(Node* head) {
	if(!head) return nullptr;
	Node* ptr = head;
	while(ptr){
		Node* newNode = new Node(ptr->val);
		newNode->next = ptr->next;
		ptr->next = newNode;

		ptr = ptr->next->next;
	}

	ptr = head;
	while(ptr){
		if(ptr->random)
			ptr->next->random = ptr->random->next;
		ptr = ptr->next->next;
	}

	ptr = head;
	Node* result = ptr->next;

	while(ptr){
		Node* temp = ptr->next;
		ptr->next = temp->next;
		if(ptr->next)
			temp->next = ptr->next->next;
		ptr = ptr->next;
	}

	return result;
}
```