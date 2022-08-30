#1.struct RuleStmt

```cpp
/* ----------------------
 *      Create Rule Statement
 * ----------------------
 */
typedef struct RuleStmt
{
    NodeTag     type;
    RangeVar   *relation;       /* relation the rule is for */
    char       *rulename;       /* name of the rule */
    Node       *whereClause;    /* qualifications */
    CmdType     event;          /* SELECT, INSERT, etc */
    bool        instead;        /* is a 'do instead'? */
    List       *actions;        /* the action statements */
    bool        replace;        /* OR REPLACE */
} RuleStmt;


```