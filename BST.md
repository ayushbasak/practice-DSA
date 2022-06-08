# Binary Search Tree
___

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

### Lowest Common Ancestor of  a Binary Search Tree [leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
```cpp
TreeNode * lowestCommonAncestor(TreeNode * root, TreeNode * p, TreeNode * q) {

	if(!root) return nullptr;

	if(p->val < root->val and q->val < root->val)
		return lowestCommonAncestor(root->left, p, q);

	if(p->val > root->val and q->val > root->val)
		return lowestCommonAncestor(root->right, p, q);

	return root;
}
```