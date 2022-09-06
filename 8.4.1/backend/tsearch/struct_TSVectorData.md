#1.struct TSVectorData

```cpp
/*
 * Structure of tsvector datatype:
 * 1) standard varlena header
 * 2) int4      size - number of lexemes or WordEntry array, which is the same
 * 3) Array of WordEntry - sorted array, comparison based on word's length
 *                          and strncmp(). WordEntry->pos points number of
 *                          bytes from end of WordEntry array to start of
 *                          corresponding lexeme.
 * 4) Lexeme's storage:
 *    lexeme (without null-terminator)
 *    if haspos is true:
 *      padding byte if necessary to make the number of positions 2-byte aligned
 *      uint16      number of positions that follow.
 *      uint16[]    positions
 *
 * The positions must be sorted.
 */

typedef struct
{
    int32       vl_len_;        /* varlena header (do not touch directly!) */
    int32       size;
    WordEntry   entries[1];     /* var size */
    /* lexemes follow */
} TSVectorData;

typedef TSVectorData *TSVector;

```