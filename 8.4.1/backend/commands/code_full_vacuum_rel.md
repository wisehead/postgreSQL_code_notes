#1.full\_vacuum_rel

```cpp
/*
 *  full_vacuum_rel() -- perform FULL VACUUM for one heap relation
 *
 *      This routine vacuums a single heap, cleans out its indexes, and
 *      updates its num_pages and num_tuples statistics.
 *
 *      At entry, we have already established a transaction and opened
 *      and locked the relation.
 *
 *      The return value indicates whether this function has held off
 *      interrupts -- caller must RESUME_INTERRUPTS() after commit if true.
 */
static bool
full_vacuum_rel(Relation onerel, VacuumStmt *vacstmt)
```