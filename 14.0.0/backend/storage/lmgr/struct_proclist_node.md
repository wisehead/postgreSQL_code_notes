#1.struct proclist_node

```cpp
/*
 * A node in a doubly-linked list of processes.  The link fields contain
 * the 0-based PGPROC indexes of the next and previous process, or
 * INVALID_PGPROCNO in the next-link of the last node and the prev-link
 * of the first node.  A node that is currently not in any list
 * should have next == prev == 0; this is not a possible state for a node
 * that is in a list, because we disallow circularity.
 */
typedef struct proclist_node
{
	int			next;			/* pgprocno of the next PGPROC */
	int			prev;			/* pgprocno of the prev PGPROC */
} proclist_node;
```