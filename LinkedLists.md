# Linked Lists
__Structure__

```cpp
struct ListNode {
	int val;
	ListNode *next;
	ListNode() : val(0), next(nullptr) {}
	ListNode(int x) : val(x), next(nullptr) {}
	ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```

### Reverse LinkedList [leetcode](https://leetcode.com/problems/reverse-linked-list/)
```cpp
ListNode* reverseList(ListNode* head) {
	ListNode* curr = head, *prev = NULL, *next = NULL;
	while(curr != NULL){
		next = curr->next;
		curr->next = prev;

		prev = curr;
		curr = next;
	}
	head = prev;
	return head;
}
```

### Swap Nodes in Pairs [leetcode](https://leetcode.com/problems/swap-nodes-in-pairs/)
```cpp
ListNode* swapPairs(ListNode* head) {
	if(!head or !head->next)
		return head;
	ListNode * ptr = head->next;
	head->next = swapPairs(head->next->next);
	ptr->next = head;
	return ptr;
}
```