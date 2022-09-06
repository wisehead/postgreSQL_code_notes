#1.struct FunctionScan

```cpp

/* ----------------
 *      FunctionScan node
 * ----------------
 */
typedef struct FunctionScan
{
    Scan        scan;
    Node       *funcexpr;       /* expression tree for func call */
    List       *funccolnames;   /* output column names (string Value nodes) */
    List       *funccoltypes;   /* OID list of column type OIDs */
    List       *funccoltypmods; /* integer list of column typmods */
} FunctionScan;
```