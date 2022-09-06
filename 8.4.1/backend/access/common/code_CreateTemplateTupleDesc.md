#1.CreateTemplateTupleDesc

```cpp

/*
 * CreateTemplateTupleDesc
 *      This function allocates an empty tuple descriptor structure.
 *
 * Tuple type ID information is initially set for an anonymous record type;
 * caller can overwrite this if needed.
 */
TupleDesc
CreateTemplateTupleDesc(int natts)
```