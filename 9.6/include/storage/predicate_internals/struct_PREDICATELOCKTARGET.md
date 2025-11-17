#1.struct PREDICATELOCKTARGET

```cpp
// The PREDICATELOCKTARGET struct represents a database object on which there are predicate locks.
typedef struct PREDICATELOCKTARGET  //在一个“对象”上的谓词锁
{
    /* hash key，一个标志，唯一表示一个对象 */
    PREDICATELOCKTARGETTAG tag; /* unique identifier of lockable object */
    /*谓词锁对象队列表 */
    SHM_QUEUE    predicateLocks; //“PREDICATELOCK”对象的队列表，其结构体如下
} PREDICATELOCKTARGET;

```