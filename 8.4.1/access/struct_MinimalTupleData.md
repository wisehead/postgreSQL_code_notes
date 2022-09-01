#1.struct MinimalTupleData

```cpp
typedef struct MinimalTupleData
{
    uint32      t_len;          /* actual length of minimal tuple */

    char        mt_padding[MINIMAL_TUPLE_PADDING];

    /* Fields below here must match HeapTupleHeaderData! */

    uint16      t_infomask2;    /* number of attributes + various flags */

    uint16      t_infomask;     /* various flag bits, see below */

    uint8       t_hoff;         /* sizeof header incl. bitmap, padding */

    /* ^ - 23 bytes - ^ */

    bits8       t_bits[1];      /* bitmap of NULLs -- VARIABLE LENGTH */

    /* MORE DATA FOLLOWS AT END OF STRUCT */
} MinimalTupleData;

typedef MinimalTupleData *MinimalTuple;
```