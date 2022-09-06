#1.struct LockMethodData

```cpp
/*
 * This data structure defines the locking semantics associated with a
 * "lock method".  The semantics specify the meaning of each lock mode
 * (by defining which lock modes it conflicts with), and also whether locks
 * of this method are transactional (ie, are released at transaction end).
 * All of this data is constant and is kept in const tables.
 *
 * numLockModes -- number of lock modes (READ,WRITE,etc) that
 *      are defined in this lock method.  Must be less than MAX_LOCKMODES.
 *
 * transactional -- TRUE if locks are released automatically at xact end.
 *
 * conflictTab -- this is an array of bitmasks showing lock
 *      mode conflicts.  conflictTab[i] is a mask with the j-th bit
 *      turned on if lock modes i and j conflict.  Lock modes are
 *      numbered 1..numLockModes; conflictTab[0] is unused.
 *
 * lockModeNames -- ID strings for debug printouts.
 *
 * trace_flag -- pointer to GUC trace flag for this lock method.
 */
typedef struct LockMethodData
{
    int         numLockModes;
    bool        transactional;
    const LOCKMASK *conflictTab;
    const char *const * lockModeNames;
    const bool *trace_flag;
} LockMethodData;

typedef const LockMethodData *LockMethod;
```