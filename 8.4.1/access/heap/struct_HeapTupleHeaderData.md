#1.struct HeapTupleHeaderData

```cpp
typedef struct HeapTupleHeaderData
{
    union
    {
        HeapTupleFields t_heap;
        DatumTupleFields t_datum;
    }           t_choice;

    ItemPointerData t_ctid;     /* current TID of this or newer tuple */

    /* Fields below here must match MinimalTupleData! */

    uint16      t_infomask2;    /* number of attributes + various flags */

    uint16      t_infomask;     /* various flag bits, see below */

    uint8       t_hoff;         /* sizeof header incl. bitmap, padding */

    /* ^ - 23 bytes - ^ */

    bits8       t_bits[1];      /* bitmap of NULLs -- VARIABLE LENGTH */

    /* MORE DATA FOLLOWS AT END OF STRUCT */
} HeapTupleHeaderData;
```