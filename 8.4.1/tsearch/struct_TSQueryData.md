#1.struct TSQueryData


```cpp
typedef struct
{
    int32       vl_len_;        /* varlena header (do not touch directly!) */
    int4        size;           /* number of QueryItems */
    char        data[1];
} TSQueryData;

typedef TSQueryData *TSQuery;

```