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

### Max Area of Island [leetcode](https://leetcode.com/problems/max-area-of-island/)
```cpp
int countIslandSize(vector<vector<int>>& grid, int r, int c){
	if(r >= 0 && c >= 0 && r < grid.size() && c < grid[0].size()
	  && grid[r][c]){
		grid[r][c] = 0;
		return 1 + countIslandSize(grid, r-1, c)
			+   countIslandSize(grid, r+1, c)
			+   countIslandSize(grid, r, c-1)
			+   countIslandSize(grid, r, c+1);
	}
	return 0;

}
int maxAreaOfIsland(vector<vector<int>>& grid) {
	int m = grid.size();
	if(!m)
		return 0;
	int n = grid[0].size();
	int maxSize = 0;
	for(int i =0; i<m; i++){
		for(int j=0; j<n; j++){
			int size = countIslandSize(grid, i, j);
			maxSize = max(maxSize, size);
		}
	}
	return maxSize;
}
```

### Word Search [leetcode](https://leetcode.com/problems/word-search/)

```cpp
bool helper(vector<vector<char>> &board, int i, int j, int R, int C, string text, int pos){
	if(i < 0 or j < 0 or i >= R or j >= C)
		return false;
	if(board[i][j] == '*') return false;

	if(board[i][j] != text[pos]) return false;

	if(pos == text.length()-1)
		return true;
	char temp = board[i][j];
	board[i][j] = '*';

	bool l,r,u,d;
	r = helper(board, i+1, j, R, C, text, pos+1); 
	l = helper(board, i-1, j, R, C, text, pos+1);
	d = helper(board, i, j+1, R, C, text, pos+1);
	u = helper(board, i, j-1, R, C, text, pos+1);

	board[i][j] = temp;

	return r or l or u or d;
}
bool exist(vector<vector<char>>& board, string word) {
	int m = board.size();
	if(!m) return false;
	int n = board[0].size();
	for(int i =0; i<m; i++){
		for(int j =0; j<n; j++)
			if(helper(board, i, j, m, n, word, 0))
				return true;
	}
	return false;
}
```