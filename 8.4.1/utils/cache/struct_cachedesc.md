#1.struct cachedesc

```cpp

/*
 *      struct cachedesc: information defining a single syscache
 */
struct cachedesc
{
    Oid         reloid;         /* OID of the relation being cached */
    Oid         indoid;         /* OID of index relation for this cache */
    int         reloidattr;     /* attr number of rel OID reference, or 0 */
    int         nkeys;          /* # of keys needed for cache lookup */
    int         key[4];         /* attribute numbers of key attrs */
    int         nbuckets;       /* number of hash buckets for this cache */
};
```