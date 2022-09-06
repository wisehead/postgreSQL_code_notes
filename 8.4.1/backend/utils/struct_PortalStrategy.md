#1.struct PortalStrategy

```cpp
/*
 * We have several execution strategies for Portals, depending on what
 * query or queries are to be executed.  (Note: in all cases, a Portal
 * executes just a single source-SQL query, and thus produces just a
 * single result from the user's viewpoint.  However, the rule rewriter
 * may expand the single source query to zero or many actual queries.)
 *
 * PORTAL_ONE_SELECT: the portal contains one single SELECT query.  We run
 * the Executor incrementally as results are demanded.  This strategy also
 * supports holdable cursors (the Executor results can be dumped into a
 * tuplestore for access after transaction completion).
 *
 * PORTAL_ONE_RETURNING: the portal contains a single INSERT/UPDATE/DELETE
 * query with a RETURNING clause (plus possibly auxiliary queries added by
 * rule rewriting).  On first execution, we run the portal to completion
 * and dump the primary query's results into the portal tuplestore; the
 * results are then returned to the client as demanded.  (We can't support
 * suspension of the query partway through, because the AFTER TRIGGER code
 * can't cope, and also because we don't want to risk failing to execute
 * all the auxiliary queries.)
 *
 * PORTAL_UTIL_SELECT: the portal contains a utility statement that returns
 * a SELECT-like result (for example, EXPLAIN or SHOW).  On first execution,
 * we run the statement and dump its results into the portal tuplestore;
 * the results are then returned to the client as demanded.
 *
 * PORTAL_MULTI_QUERY: all other cases.  Here, we do not support partial
 * execution: the portal's queries will be run to completion on first call.
 */
typedef enum PortalStrategy
{
    PORTAL_ONE_SELECT,
    PORTAL_ONE_RETURNING,
    PORTAL_UTIL_SELECT,
    PORTAL_MULTI_QUERY
} PortalStrategy;

```