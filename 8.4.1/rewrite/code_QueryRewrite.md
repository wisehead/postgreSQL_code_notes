#1.QueryRewrite

```cpp
/*
 * QueryRewrite -
 *    Primary entry point to the query rewriter.
 *    Rewrite one query via query rewrite system, possibly returning 0
 *    or many queries.
 *
 * NOTE: the parsetree must either have come straight from the parser,
 * or have been scanned by AcquireRewriteLocks to acquire suitable locks.
 */
List *
QueryRewrite(Query *parsetree)

```

#2.caller

- pg_rewrite_query
- PrepareQuery