#1.lazy_vacuum_rel

```cpp
/*
 *  lazy_vacuum_rel() -- perform LAZY VACUUM for one heap relation
 *
 *      This routine vacuums a single heap, cleans out its indexes, and
 *      updates its relpages and reltuples statistics.
 *
 *      At entry, we have already established a transaction and opened
 *      and locked the relation.
 *
 *      The return value indicates whether this function has held off
 *      interrupts -- caller must RESUME_INTERRUPTS() after commit if true.
 */
bool
lazy_vacuum_rel(Relation onerel, VacuumStmt *vacstmt,
                BufferAccessStrategy bstrategy)
```