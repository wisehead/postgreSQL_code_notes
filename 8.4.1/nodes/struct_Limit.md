#1.struct Limit

```cpp

/* ----------------
 *      limit node
 *
 * Note: as of Postgres 8.2, the offset and count expressions are expected
 * to yield int8, rather than int4 as before.
 * ----------------
 */
typedef struct Limit
{
    Plan        plan;
    Node       *limitOffset;    /* OFFSET parameter, or NULL if none */
    Node       *limitCount;     /* COUNT parameter, or NULL if none */
} Limit;
```