#1.struct FuncCall

```cpp
/*
 * FuncCall - a function or aggregate invocation
 *
 * agg_star indicates we saw a 'foo(*)' construct, while agg_distinct
 * indicates we saw 'foo(DISTINCT ...)'.  In either case, the construct
 * *must* be an aggregate call.  Otherwise, it might be either an
 * aggregate or some other kind of function.  However, if OVER is present
 * it had better be an aggregate or window function.
 */
typedef struct FuncCall
{
    NodeTag     type;
    List       *funcname;       /* qualified name of function */
    List       *args;           /* the arguments (list of exprs) */
    bool        agg_star;       /* argument was really '*' */
    bool        agg_distinct;   /* arguments were labeled DISTINCT */
    bool        func_variadic;  /* last argument was labeled VARIADIC */
    struct WindowDef *over;     /* OVER clause, if any */
    int         location;       /* token location, or -1 if unknown */
} FuncCall;
```