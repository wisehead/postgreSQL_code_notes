#1.pg_advisory_lock_int8

```cpp
pg_advisory_lock_int8(PG_FUNCTION_ARGS) //在指定对象上加排它式的劝告锁
{...
    SET_LOCKTAG_INT64(tag, key);
    (void) LockAcquire(&tag, ExclusiveLock, true, false); //第四个参数值为true，
    表示劝告锁是Session级的锁，由用户通过函数直接施加。第四个参数值为false，表示获取不到锁
    就不等待
...}
```