#1.enum LockTupleMode

```cpp
typedef enum LockTupleMode  //元组级锁的模式
{
    LockTupleKeyShare,  /* SELECT FOR KEY SHARE */
    LockTupleShare,     /* SELECT FOR SHARE */
    LockTupleNoKeyExclusive,  /* SELECT FOR NO KEY UPDATE, and UPDATEs that don't
    modify key columns */  //UPDATE操作只修改非索引列
    LockTupleExclusive        /* SELECT FOR UPDATE, UPDATEs that modify key
    columns, and DELETE */  //UPDATE操作修改索引列
} LockTupleMode;
```