#1.GetLockStatusData

```cpp
4.为对象加锁
GetLockStatusData(void)
{...
    for (i = 0; i < ProcGlobal->allProcCount; ++i)
    {...
        for (f = 0; f < FP_LOCK_SLOTS_PER_BACKEND; ++f)
        {
            LockInstanceData *instance;
...
            instance = &data->locks[el];
            SET_LOCKTAG_RELATION(instance->locktag, proc->databaseId, proc->fpRelId[f]);
            instance->holdMask = LOCKBIT_ON(ExclusiveLock);
...
        }
...
    }
...
}

```