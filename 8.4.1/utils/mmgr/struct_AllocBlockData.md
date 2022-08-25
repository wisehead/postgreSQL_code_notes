#1.struct AllocBlockData

```cpp
/*
 * AllocBlock
 *      An AllocBlock is the unit of memory that is obtained by aset.c
 *      from malloc().  It contains one or more AllocChunks, which are
 *      the units requested by palloc() and freed by pfree().  AllocChunks
 *      cannot be returned to malloc() individually, instead they are put
 *      on freelists by pfree() and re-used by the next palloc() that has
 *      a matching request size.
 *
 *      AllocBlockData is the header data for a block --- the usable space
 *      within the block begins at the next alignment boundary.
 */
typedef struct AllocBlockData
{
    AllocSet    aset;           /* aset that owns this block */
    AllocBlock  next;           /* next block in aset's blocks list */
    char       *freeptr;        /* start of free space in this block */
    char       *endptr;         /* end of space in this block */
} AllocBlockData;
```