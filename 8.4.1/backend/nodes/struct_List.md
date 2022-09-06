#1.struct List

```cpp


typedef struct List
{
    NodeTag     type;           /* T_List, T_IntList, or T_OidList */
    int         length;
    ListCell   *head;
    ListCell   *tail;
} List;

struct ListCell
{
    union
    {
        void       *ptr_value;
        int         int_value;
        Oid         oid_value;
    }           data;
    ListCell   *next;
};

```