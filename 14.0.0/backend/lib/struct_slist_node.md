#1.struct slist_node

```cpp
/*
 * Node of a singly linked list.
 *
 * Embed this in structs that need to be part of a singly linked list.
 */
typedef struct slist_node slist_node;
struct slist_node
{
	slist_node *next;
};

```