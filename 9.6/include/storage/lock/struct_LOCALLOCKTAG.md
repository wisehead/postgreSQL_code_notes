#1.struct LOCALLOCKTAG

```cpp
typedef struct LOCALLOCKTAG   //锁对象上加什么粒度的锁
{
    LOCKTAG     lock;    /* identifies the lockable object */  //锁对象上
    LOCKMODE    mode;    /* lock mode for this table entry */  //加什么粒度的锁
} LOCALLOCKTAG;
```