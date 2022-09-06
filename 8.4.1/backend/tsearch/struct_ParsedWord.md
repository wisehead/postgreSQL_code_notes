#1.struct ParsedWord

```cpp
/*
 * parse plain text and lexize words
 */
typedef struct
{
    uint16      len;
    uint16      nvariant;
    union
    {
        uint16      pos;

        /*
         * When apos array is used, apos[0] is the number of elements in the
         * array (excluding apos[0]), and alen is the allocated size of the
         * array.
         */
        uint16     *apos;
    }           pos;
    uint16      flags;          /* currently, only TSL_PREFIX */
    char       *word;
    uint32      alen;
} ParsedWord;

```