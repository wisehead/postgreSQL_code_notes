#1.heap_tableam_handler

```cpp
Datum
heap_tableam_handler(PG_FUNCTION_ARGS)
{
    PG_RETURN_POINTER(&heapam_methods);
}
```

#2.caller

```cpp

const FmgrBuiltin fmgr_builtins[] = {
  { 3, 1, true, false, "heap_tableam_handler", heap_tableam_handler },
  { 31, 1, true, false, "byteaout", byteaout },
  { 33, 1, true, false, "charout", charout },
  { 34, 1, true, false, "namein", namein },
  ...
}
```