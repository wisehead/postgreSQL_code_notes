#1.transformCreateStmt

```cpp

/*
 * transformCreateStmt -
 *    parse analysis for CREATE TABLE
 *
 * Returns a List of utility commands to be done in sequence.  One of these
 * will be the transformed CreateStmt, but there may be additional actions
 * to be done before and after the actual DefineRelation() call.
 *
 * SQL92 allows constraints to be scattered all over, so thumb through
 * the columns and collect all constraints into one place.
 * If there are any implied indices (e.g. UNIQUE or PRIMARY KEY)
 * then expand those into multiple IndexStmt blocks.
 *    - thomas 1997-12-02
 */
List *
transformCreateStmt(CreateStmt *stmt, const char *queryString)
```