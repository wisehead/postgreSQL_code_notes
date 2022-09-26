#1.FormIndexDatum

```
FormIndexDatum
--if (indexInfo->ii_Expressions != NIL &&
		indexInfo->ii_ExpressionsState == NIL)
----indexInfo->ii_ExpressionsState =
			ExecPrepareExprList(indexInfo->ii_Expressions, estate);
```