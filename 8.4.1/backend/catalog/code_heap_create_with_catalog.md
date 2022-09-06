#1.heap_create_with_catalog

```cpp
/* --------------------------------
 *      heap_create_with_catalog
 *
 *      creates a new cataloged relation.  see comments above.
 * --------------------------------
 */
Oid
heap_create_with_catalog(const char *relname,
                         Oid relnamespace,
                         Oid reltablespace,
                         Oid relid,
                         Oid ownerid,
                         TupleDesc tupdesc,
                         List *cooked_constraints,
                         char relkind,
                         bool shared_relation,
                         bool oidislocal,
                         int oidinhcount,
                         OnCommitAction oncommit,
                         Datum reloptions,
                         bool allow_system_table_mods)

```