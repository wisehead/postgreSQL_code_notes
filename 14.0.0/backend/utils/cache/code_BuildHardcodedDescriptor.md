#1.BuildHardcodedDescriptor

```
BuildHardcodedDescriptor
--CreateTemplateTupleDesc
--for (i = 0; i < natts; i++)
----memcpy(TupleDescAttr(result, i), &attrs[i], ATTRIBUTE_FIXED_PART_SIZE);
----TupleDescAttr(result, i)->attcacheoff = -1;
--TupleDescAttr(result, 0)->attcacheoff = 0;
```