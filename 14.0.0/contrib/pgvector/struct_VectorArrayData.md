#1.struct VectorArrayData

```cpp
typedef struct VectorArrayData
{
    int         length;
    int         maxlen;
    int         dim;
    Vector      items[FLEXIBLE_ARRAY_MEMBER];
}           VectorArrayData;

typedef VectorArrayData * VectorArray;

```