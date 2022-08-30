#1.fireRIRrules

```cpp

/*
 * fireRIRrules -
 *  Apply all RIR rules on each rangetable entry in a query
 */
static Query *
fireRIRrules(Query *parsetree, List *activeRIRs)
```

#2.caller
- QueryRewrite
- fireRIRonSubLink
- ApplyRetrieveRule