#1.struct A_Expr

```cpp
typedef struct A_Expr
{
    NodeTag     type;
    A_Expr_Kind kind;           /* see above */
    List       *name;           /* possibly-qualified name of operator */
    Node       *lexpr;          /* left argument, or NULL if none */
    Node       *rexpr;          /* right argument, or NULL if none */
    int         location;       /* token location, or -1 if unknown */
} A_Expr;

```