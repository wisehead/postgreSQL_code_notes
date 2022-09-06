#1.struct EDGE

```cpp
/* One edge in the waits-for graph */
typedef struct
{
    PGPROC     *waiter;         /* the waiting process */
    PGPROC     *blocker;        /* the process it is waiting for */
    int         pred;           /* workspace for TopoSort */
    int         link;           /* workspace for TopoSort */
} EDGE;
```