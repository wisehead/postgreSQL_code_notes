#1.struct Expr

```cpp
/* ----------------------------------------------------------------
 *                  node types for executable expressions
 * ----------------------------------------------------------------
 */

/*
 * Expr - generic superclass for executable-expression nodes
 *
 * All node types that are used in executable expression trees should derive
 * from Expr (that is, have Expr as their first field).  Since Expr only
 * contains NodeTag, this is a formality, but it is an easy form of
 * documentation.  See also the ExprState node types in execnodes.h.
 */
typedef struct Expr
{
    NodeTag     type;
} Expr;
```