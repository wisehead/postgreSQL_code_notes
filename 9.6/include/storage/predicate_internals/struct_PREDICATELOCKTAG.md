#1.struct PREDICATELOCKTAG

```cpp
typedef struct PREDICATELOCKTAG
//谓词锁的标识。把一个数据库对象、对象上的谓词锁和一个事务绑定在一起
{
    PREDICATELOCKTARGET  *myTarget;  //用一个四元组表示一个数据库对象
    SERIALIZABLEXACT     *myXact;    //一个序列化的事务
} PREDICATELOCKTAG;

```