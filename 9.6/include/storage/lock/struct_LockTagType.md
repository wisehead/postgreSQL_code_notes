#1.struct LockTagType

```cpp
/* LOCKTAG is the key information needed to look up a LOCK item in the lock hashtable.
 * A LOCKTAG value uniquely identifies a lockable object.
 *
 * The LockTagType enum defines the different kinds of objects we can lock. We can
 handle up to 256 different LockTagTypes.
 */
typedef enum LockTagType  //枚举了可以加锁的对象，同时也能表明锁的类型。各个具体的枚举值的
更详细说明，参见表8-8
{
    LOCKTAG_RELATION,  /* whole relation */ /*在关系（表、视图、索引等）上加
“RELATION”锁，主要用于CREATE、DROP、TRUNCATE、VACUUM等操作。可见这是一个元数据类型的锁，
类似MySQL的MDL_lock锁*/
    /* ID info for a relation is DB OID + REL OID; DB OID = 0 if shared */
    LOCKTAG_RELATION_EXTEND,  /* the right to extend a relation */
    //物理文件因存储空间不够做扩展，如表的数据的增加会导致文件变大
    /* same ID info as RELATION */
    LOCKTAG_PAGE,  /* one page of a relation */
    //在物理页面上加“PAGE”锁，主要用于索引相关的INSERT、DELETE操作
    /* ID info for a page is RELATION info + BlockNumber */
    LOCKTAG_TUPLE, /* one physical tuple */
    //在物理元组上加“TUPLE”锁，主要用于INSERT、UPDATE操作，实际是行级锁
    /* ID info for a tuple is PAGE info + OffsetNumber */
    LOCKTAG_TRANSACTION,  /* transaction (for waiting for xact done) */
    //事务的锁
    /* ID info for a transaction is its TransactionId */
    LOCKTAG_VIRTUALTRANSACTION, /* virtual transaction (ditto) */
    //虚拟事务的锁
    /* ID info for a virtual transaction is its VirtualTransactionId */
    LOCKTAG_SPECULATIVE_TOKEN,    /* speculative insertion Xid and token */
    //事务的锁
    /* ID info for a transaction is its TransactionId */
    LOCKTAG_OBJECT,                /* non-relation database object */
    //非“RELATION”锁，如database
    /* ID info for an object is DB OID + CLASS OID + OBJECT OID + SUBID */
    /* Note: object ID has same representation as in pg_depend and pg_description,
     * but notice that we are constraining SUBID to 16 bits.
     * Also, we use DB OID = 0 for shared objects such as tablespaces.  */
    LOCKTAG_USERLOCK,            /* reserved for old contrib/userlock code */
    //用户锁
    LOCKTAG_ADVISORY         /* advisory user locks */  //劝告锁，供用户显式加锁使用
} LockTagType;
```