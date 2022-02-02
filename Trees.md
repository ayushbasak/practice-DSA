# Trees

LeetCode / InterviewBit Questions based on Trees 

#### Inorder Traversal [leetcode](https://leetcode.com/problems/binary-tree-inorder-traversal/)
```cpp
void inorder(TreeNode * root){
	if(!root) return;
	stack<TreeNode *> st;
	
	while(root or !st.empty()){
		while(root){
			st.push(root);
			root = root->left;
		}
		
		root = st.pop();
		cout << root->val;
		root = root->right;
	}
}
```

### Valid BST [leetcode](https://leetcode.com/problems/validate-binary-search-tree/)
```cpp
bool isValidBST(TreeNode * root){
	if(!root) return true;
	stack<TreeNode * > st;
	TreeNode * previous = nullptr;
	while(root or !st.empty()){
		while(root){
			st.push(root);
			root = root->left;
		}

		root = st.top(); st.pop();
		if(previous and root->val <= previous->val)
			return false;

		previous = root;
		root = root->right;
	}
	return true;
}
```

---
###  Next Pointer binary Tree [leetcode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)
```cpp
void connect(TreeLinkNode * root){
	if(!root) return;
	TreeLinkNode * currLevel = root;
	
	while(currLevel){
		TreeLinkNode * curr = currLevel;
		while(curr){
			if(curr->left)
				curr->left->next = curr->right;
			if(curr->right and curr->next)
				curr->right->next = curr->next->left;
			
			curr = curr->next;
		}
		currLevel = currLevel->left;
	}
}
```

### Nodes at distance K [leetcode](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)
```cpp
vector<int> distanceK(TreeNode * root, TreeNode * target, int K){
	unordered_map<TreeNode *, TreeNode *> parent;
	unordered_map<TreeNode *, bool> visited;
	queue<TreeNode *> q;
	
	q.push(root);
	while(!q.empty()){
		TreeNode * curr = q.front(); q.pop();
		if(curr->left){
			q.push(curr->left);
			parent[curr->left] = curr;
		}
		if(curr->right){
			q.push(curr->right);
			parent[curr->right] = curr;
		}
		
	}
	
	q.push(target);
	visited[target] = true;
	int level = 0;
	while(!q.empty()){
		int size = q.size();
		if(level == K)
			break;
		for(int i =0; i < size; i++){
			TreeNode * curr = q.front(); q.pop();
			if(curr->left and !visited[curr->left]){
				q.push(curr->left);
				visited[curr->left] = true;
			}
			if(curr->right and !visited[curr->right]){
				q.push(curr->right);
				visited[curr->right] = true;
			}
			if(parent[curr] and !visited[parent[curr]]){
				q.push(parent[curr]);
				visited[parent[curr]] = true;
			}
		}
		
		level++;
	}
	vector<int> result;
	while(!q.empty()){
		TreeNode * curr = q.front(); q.pop();
		result.push_back(curr->val);
	}
	return result;
}
```