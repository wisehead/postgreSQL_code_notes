#1.ProcessQuery

```cpp
/*
 * ProcessQuery
 *      Execute a single plannable query within a PORTAL_MULTI_QUERY
 *      or PORTAL_ONE_RETURNING portal
 *
 *  plan: the plan tree for the query
 *  sourceText: the source text of the query
 *  params: any parameters needed
 *  dest: where to send results
 *  completionTag: points to a buffer of size COMPLETION_TAG_BUFSIZE
 *      in which to store a command completion status string.
 *
 * completionTag may be NULL if caller doesn't want a status string.
 *
 * Must be called in a memory context that will be reset or deleted on
 * error; otherwise the executor's memory usage will be leaked.
 */
static void
ProcessQuery(PlannedStmt *plan,
             const char *sourceText,
             ParamListInfo params,
             DestReceiver *dest,
             char *completionTag)
```