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

### Invert Binary Tree [leetcode](https://leetcode.com/problems/invert-binary-tree/)
```cpp
TreeNode* invertTree(TreeNode* root) {
	if(!root) return nullptr;
	TreeNode * temp = root->left;
	root->left = root->right;
	root->right = temp; 
	
	invertTree(root->left);
	invertTree(root->right);
	
	return root;
}
```

### Level Order [leetcode](https://leetcode.com/problems/binary-tree-level-order-traversal/)
```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
	if(!root)
		return {};

	queue<TreeNode *> q;
	vector<vector<int>> res;
	q.push(root);

	while(!q.empty()){
		int size = q.size();
		vector<int> temp;
		for(int i =0; i<size; i++){
			TreeNode * top = q.front();
			q.pop();
			temp.push_back(top->val);

			if(top->left)
				q.push(top->left);
			if(top->right)
				q.push(top->right);
		}
		res.push_back(temp);
	}
	return res;
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

### Merge Two binary trees [leetcode](https://leetcode.com/problems/merge-two-binary-trees/)
```cpp
TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
	if(root1 and root2){
		TreeNode * root = new TreeNode(root1->val+root2->val);
		root->left = mergeTrees(root1->left, root2->left);
		root->right = mergeTrees(root1->right, root2->right);

		return root;
	}
	return root1 ? root1 : root2;
}
```

### Symmetric Tree [leetcode](https://leetcode.com/problems/symmetric-tree/)
```cpp
bool isSymmetric(TreeNode * root){
	if(root)
		return helper(root->left, root->right);
	return false;
}
bool helper(TreeNode * left, TreeNode * right){
	if(!left or !right)
		return left == right;
	
	if(left->val != right->val)
		return false;	
	return helper(left->left, right->right) and helper(left->right, 	right->left);
}
```

### Flatten Binary Tree into Linked List [leetcode](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)
```cpp
void flatten(TreeNode * root){
	if(!root) return;
	
	flatten(root->left);
	flatten(root->right);
	
	if(root->left){
		TreeNode * temp  = root->right;
		root->right = root->left;
		
		root->left = nullptr;
		while(root->right)
			root = root->right;
		
		root->right = temp;
	}
}
```

### Maximum Binary Tree [leetcode](https://leetcode.com/problems/maximum-binary-tree/)
```cpp
TreeNode * helper(vector<int> &nums, int start, int end){
	if(start > end) 
		return nullptr;
	int curr_max = 0;
	int index = 0;
	for(int i =start; i<= end; i++){
		if(curr_max <= nums[i]){
			curr_max = nums[i];
			index = i;
		}
	}

	TreeNode * newNode = new TreeNode(curr_max);
	newNode->left = helper(nums, start, index - 1);
	newNode->right = helper(nums, index + 1, end);

	return newNode;
}
TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
	return helper(nums, 0, nums.size() - 1);
}
```

### Diameter of Binary Tree [leetcode](https://leetcode.com/problems/diameter-of-binary-tree/)
```cpp
int curr_max = 0;
int diameterOfBinaryTree(TreeNode* root) {
	traverse(root);
	return curr_max;
}
int traverse(TreeNode * node){
	if(!node)
		return 0;

	int left = traverse(node->left);
	int right = traverse(node->right);

	curr_max = max(curr_max, left + right);

	return max(left, right) + 1;   
}
```

### Balanced Binary Tree [leetcode](https://leetcode.com/problems/balanced-binary-tree/)
```cpp
int helper(TreeNode * root) {
	if (!root) return 0;

	int left_height = helper(root -> left);
	int right_height = helper(root -> right);
	if (left_height < 0 or right_height < 0) 
		return -1;

	if (abs(left_height - right_height) > 1)  
		return -1;
	return max (left_height, right_height) + 1;
}
bool isBalanced(TreeNode *root) {
	return dfsHeight (root) != -1;
}
```
