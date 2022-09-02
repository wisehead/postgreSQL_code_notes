#1.TupleDescInitEntry

```cpp
/*
 * TupleDescInitEntry
 *      This function initializes a single attribute structure in
 *      a previously allocated tuple descriptor.
 *
 * If attributeName is NULL, the attname field is set to an empty string
 * (this is for cases where we don't know or need a name for the field).
 * Also, some callers use this function to change the datatype-related fields
 * in an existing tupdesc; they pass attributeName = NameStr(att->attname)
 * to indicate that the attname field shouldn't be modified.
 *
 * Note that attcollation is set to the default for the specified datatype.
 * If a nondefault collation is needed, insert it afterwards using
 * TupleDescInitEntryCollation.
 */
void
TupleDescInitEntry(TupleDesc desc,
                   AttrNumber attributeNumber,
                   const char *attributeName,
                   Oid oidtypeid,
                   int32 typmod,
                   int attdim)
```