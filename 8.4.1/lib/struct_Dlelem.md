#1.struct Dlelem

```cpp
typedef struct Dlelem
{
    struct Dlelem *dle_next;    /* next element */
    struct Dlelem *dle_prev;    /* previous element */
    void       *dle_val;        /* value of the element */
    struct Dllist *dle_list;    /* what list this element is in */
} Dlelem;
```