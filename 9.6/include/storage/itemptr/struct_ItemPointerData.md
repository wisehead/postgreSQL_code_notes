#1.struct ItemPointerData

```cpp
typedef struct ItemPointerData
{
    BlockIdData ip_blkid;   //32位的块地址，分为bi_hi和bi_lo两部分，表示一个物理页面的地址
    OffsetNumber ip_posid;  //16位的无符号数，表示在一个页面内部的偏移量
}
```