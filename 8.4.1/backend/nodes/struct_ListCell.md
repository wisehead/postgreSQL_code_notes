#1.struct ListCell

```cpp
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