#1.struct FmgrInfo

```cpp
/*
 * This struct holds the system-catalog information that must be looked up
 * before a function can be called through fmgr.  If the same function is
 * to be called multiple times, the lookup need be done only once and the
 * info struct saved for re-use.
 *
 * Note that fn_expr really is parse-time-determined information about the
 * arguments, rather than about the function itself.  But it's convenient to
 * store it here rather than in FunctionCallInfoBaseData, where it might more
 * logically belong.
 *
 * fn_extra is available for use by the called function; all other fields
 * should be treated as read-only after the struct is created.
 */
typedef struct FmgrInfo
{
    PGFunction  fn_addr;        /* pointer to function or handler to be called */
    Oid         fn_oid;         /* OID of function (NOT of handler, if any) */
    short       fn_nargs;       /* number of input args (0..FUNC_MAX_ARGS) */
    bool        fn_strict;      /* function is "strict" (NULL in => NULL out) */
    bool        fn_retset;      /* function returns a set */
    unsigned char fn_stats;     /* collect stats if track_functions > this */
    void       *fn_extra;       /* extra space for use by handler */
    MemoryContext fn_mcxt;      /* memory context to store fn_extra in */
    fmNodePtr   fn_expr;        /* expression parse tree for call, or NULL */
} FmgrInfo;
```