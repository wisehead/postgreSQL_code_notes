#1.struct Vector

```cpp
typedef struct Vector
{
    int32       vl_len_;        /* varlena header (do not touch directly!) */
    int16       dim;            /* number of dimensions */
    int16       unused;
    float       x[FLEXIBLE_ARRAY_MEMBER];
}           Vector;
```