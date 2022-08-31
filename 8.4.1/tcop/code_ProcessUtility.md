#1.ProcessUtility

```cpp
/*
 * ProcessUtility
 *      general utility function invoker
 *
 *  parsetree: the parse tree for the utility statement
 *  queryString: original source text of command
 *  params: parameters to use during execution
 *  isTopLevel: true if executing a "top level" (interactively issued) command
 *  dest: where to send results
 *  completionTag: points to a buffer of size COMPLETION_TAG_BUFSIZE
 *      in which to store a command completion status string.
 *
 * Notes: as of PG 8.4, caller MUST supply a queryString; it is not
 * allowed anymore to pass NULL.  (If you really don't have source text,
 * you can pass a constant string, perhaps "(query not available)".)
 *
 * completionTag is only set nonempty if we want to return a nondefault status.
 *
 * completionTag may be NULL if caller doesn't want a status string.
 */
void
ProcessUtility(Node *parsetree,
               const char *queryString,
               ParamListInfo params,
               bool isTopLevel,
               DestReceiver *dest,
               char *completionTag)
```