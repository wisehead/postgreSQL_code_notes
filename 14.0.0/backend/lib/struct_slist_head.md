#1.struct slist_head

```cpp
/*
 * Head of a singly linked list.
 *
 * Singly linked lists are not circularly linked, in contrast to doubly linked
 * lists; we just set head.next to NULL if empty.  This doesn't incur any
 * additional branches in the usual manipulations.
 */
typedef struct slist_head
{
	slist_node	head;
} slist_head;
```