#1.struct HeapTupleHeaderData

```cpp

struct HeapTupleHeaderData  //元组头
{
    union
    {
        HeapTupleFields t_heap; //t_xmin和t_xmax等系统列
        DatumTupleFields t_datum;
    } t_choice;
    ItemPointerData t_ctid;  /* current TID of this or newer tuple (or a *
 speculative insertion token) */ /*记录当前元组或者新元组的物理位置（物理位置是指：块内偏
移和元组长度）。如果元组被更新（逻辑上做删除标记，表示删除旧版本元组，并插入新版本元组），则t_
ctid记录的是新版本元组的物理位置*/
    /* Fields below here must match MinimalTupleData! */
    //两个重要的标志位，htup_details.h文件中定义了每个标志位可设置的标志
    uint16        t_infomask2;    /* number of attributes + various flags */
    //低11位表示当前元组的属性个数。高5位用于包括用于HOT技术及元组可见性的标志位
    uint16        t_infomask;     /* various flag bits, see below */
    //用于标识元组当前的状态。如元组是否具有OID、是否有空属性等，t_infomask的每一位对应不同的
    状态，共21种状态（代码中查找“HEAP_HASNULL”等）如下所列
    uint8         t_hoff;         /* sizeof header incl. bitmap, padding */
    //元组头的大小
    /* ^ - 23 bytes - ^ */
    bits8         t_bits[FLEXIBLE_ARRAY_MEMBER];    /* bitmap of NULLs */
    //标识该元组的哪些字段为空
    /* MORE DATA FOLLOWS AT END OF STRUCT */        //本结构后，将紧跟着数据
};
```