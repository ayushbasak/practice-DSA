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

### Parition List [leetcode](https://leetcode.com/problems/partition-list/)
```cpp
ListNode * partition(ListNode * head, int x) {
	ListNode n1(0), n2(0);
	ListNode * p1 = &n1, * p2 = &n2;
	while(head){
		if(head->val >= x)
		p2 = p2->next = head;
		else
		p1 = p1->next = head;
		head = head->next;
	}
	p2->next = nullptr;
	p1->next = n2.next;
	return n1.next;
}
```

### Insertion Sort List [leetcode](https://leetcode.com/problems/insertion-sort-list/)

```cpp
    ListNode* insertionSortList(ListNode* head) {
        if(!head) return nullptr;
        
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode *prev = dummy, *ptr = head;
        while (ptr) {
            if (ptr->next and ptr->next->val < ptr->val) {
                while (prev->next and prev->next->val < ptr->next->val)
                    prev = prev -> next;
                ListNode* temp = prev->next;
                prev->next = ptr->next;
                ptr->next = ptr->next->next;
                prev->next->next = temp;
                prev = dummy;
            }
            else 
                ptr = ptr->next;
        }
        return dummy -> next;
    }
```