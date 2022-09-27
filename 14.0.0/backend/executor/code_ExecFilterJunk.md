#1.ExecFilterJunk

```
ExecFilterJunk
--slot_getallattrs(slot);
----slot_getsomeattrs(slot, slot->tts_tupleDescriptor->natts);
------slot_getsomeattrs_int
-------tts_heap_getsomeattrs
---------slot_deform_heap_tuple
--old_values = slot->tts_values;
--old_isnull = slot->tts_isnull;
--cleanTupType = junkfilter->jf_cleanTupType;   
--cleanLength = cleanTupType->natts;            
--cleanMap = junkfilter->jf_cleanMap;           
--resultSlot = junkfilter->jf_resultSlot;       
--ExecClearTuple(resultSlot);
--values = resultSlot->tts_values;
--isnull = resultSlot->tts_isnull;
--for (i = 0; i < cleanLength; i++)
	{
		int			j = cleanMap[i];

		if (j == 0)
		{
			values[i] = (Datum) 0;
			isnull[i] = true;
		}
		else
		{
			values[i] = old_values[j - 1];
			isnull[i] = old_isnull[j - 1];
		}
	}
--return ExecStoreVirtualTuple(resultSlot);
```