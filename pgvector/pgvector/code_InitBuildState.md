#1.InitBuildState

```cpp
InitBuildState
--IvfflatGetLists
----*opts = (IvfflatOptions *) index->rd_options;
--buildstate->dimensions = TupleDescAttr(index->rd_att, 0)->atttypmod;
```

#2.caller

- BuildIndex