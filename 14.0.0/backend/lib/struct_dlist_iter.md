#1.struct dlist_iter

```cpp
/*
 * Doubly linked list iterator.
 *
 * Used as state in dlist_foreach() and dlist_reverse_foreach(). To get the
 * current element of the iteration use the 'cur' member.
 *
 * Iterations using this are *not* allowed to change the list while iterating!
 *
 * NB: We use an extra "end" field here to avoid multiple evaluations of
 * arguments in the dlist_foreach() macro.
 */
typedef struct dlist_iter
{
	dlist_node *cur;			/* current element */
	dlist_node *end;			/* last node we'll iterate to */
} dlist_iter;

```