#1.transformTargetList

```cpp
/*
 * transformTargetList()
 * Turns a list of ResTarget's into a list of TargetEntry's.
 *
 * At this point, we don't care whether we are doing SELECT, INSERT,
 * or UPDATE; we just transform the given expressions (the "val" fields).
 */
List *
transformTargetList(ParseState *pstate, List *targetlist)
```

#2.caller

- transformSelectStmt
- transformUpdateStmt
- transformReturningList