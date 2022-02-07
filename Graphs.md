# Graph Algorithms
### Clone Graph [leetcode](https://leetcode.com/problems/clone-graph/submissions/)

__DFS__ 
```cpp
unordered_map<Node *, Node *> mp;
Node* cloneGraph(Node* node) {
	if(!node) return nullptr;
	if(mp[node])
		return mp[node];
	mp[node] = new Node(node->val, {});
	for(Node * neighbor: node->neighbors){
		mp[node]->neighbors.push_back( cloneGraph(neighbor) );
	}
	return mp[node];
}
```
