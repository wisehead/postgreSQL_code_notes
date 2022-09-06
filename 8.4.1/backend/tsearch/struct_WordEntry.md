#1.struct WordEntry

```cpp
/*
 * TSVector type.
 * Note, tsvectorsend/recv believe that sizeof(WordEntry) == 4
 */

typedef struct
{
    uint32
                haspos:1,
                len:11,         /* MAX 2Kb */
                pos:20;         /* MAX 1Mb */
} WordEntry;

```