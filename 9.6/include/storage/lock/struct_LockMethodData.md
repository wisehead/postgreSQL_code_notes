#1.struct LockMethodData

```cpp
typedef struct LockMethodData   //锁方法表
{
    int  numLockModes;     //一个锁方法表里，锁模式的个数。系统自定义了两个锁方法表，都是8
    个锁模式（采用宏AccessExclusiveLock的值）
    const LOCKMASK    *conflictTab;    //锁的相容性列表，对应了“default_lockmethod”
    和“user_lockmethod”中都使用了的“LockConflicts”
    const char *const * lockModeNames; //锁方法名称
    const bool        *trace_flag;     //用于调试目的的跟踪信息
} LockMethodData;
typedef const LockMethodData *LockMethod;

```