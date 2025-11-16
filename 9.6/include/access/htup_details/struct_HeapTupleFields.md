#1.struct HeapTupleFields

```cpp
typedef struct HeapTupleFields
{
    //表示一个事务的生命段[t_xmin, t_xmax]，t_xmin生于何时，t_xmax卒于何时
    TransactionId t_xmin;  /* inserting xact ID */
    //本条元组是由哪个事务创建的（创建者一定时INSERT/COPY...FROM类操作）
    TransactionId t_xmax;  /* deleting or locking xact ID */
    //本条元组是由哪个事务删除的
    union
    {
        CommandId     t_cid;     /* inserting or deleting command ID, or both */
        TransactionId t_xvac;    /* old-style VACUUM FULL xact ID */
    }  t_field3;
} HeapTupleFields;

```