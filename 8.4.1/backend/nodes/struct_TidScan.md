#1.struct TidScan

```cpp

/* ----------------
 *      tid scan node
 *
 * tidquals is an implicitly OR'ed list of qual expressions of the form
 * "CTID = pseudoconstant" or "CTID = ANY(pseudoconstant_array)".
 * ----------------
 */
typedef struct TidScan
{
    Scan        scan;
    List       *tidquals;       /* qual(s) involving CTID = something */
} TidScan;
```