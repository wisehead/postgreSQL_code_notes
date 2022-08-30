#1.DefineQueryRewrite

```cpp
/*
 * DefineQueryRewrite
 *      Create a rule
 *
 * This is essentially the same as DefineRule() except that the rule's
 * action and qual have already been passed through parse analysis.
 */
void
DefineQueryRewrite(char *rulename,
                   Oid event_relid,
                   Node *event_qual,
                   CmdType event_type,
                   bool is_instead,
                   bool replace,
                   List *action)

```

#2.caller

- DefineRule
- DefineViewRules