#1.struct MemoryContextData

```cpp

typedef struct MemoryContextData
{
    NodeTag     type;           /* identifies exact kind of context */
    MemoryContextMethods *methods;      /* virtual function table */
    MemoryContext parent;       /* NULL if no parent (toplevel context) */
    MemoryContext firstchild;   /* head of linked list of children */
    MemoryContext nextchild;    /* next child of same parent */
    char       *name;           /* context name (just for debugging) */
} MemoryContextData;
```