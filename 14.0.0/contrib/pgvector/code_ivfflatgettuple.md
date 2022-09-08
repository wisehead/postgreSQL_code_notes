#1. ivfflatgettuple

```cpp
ivfflatgettuple
--if (so->first)
--//not first time
--tuplesort_gettupleslot(so->sortstate, true, false, so->slot, NULL)
--
```