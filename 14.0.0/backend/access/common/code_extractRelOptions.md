#1.extractRelOptions

```
extractRelOptions
--datum = fastgetattr(tuple,
						Anum_pg_class_reloptions,
						tupdesc,
						&isnull);
--classForm = (Form_pg_class) GETSTRUCT(tuple);
--switch (classForm->relkind)
	{
		case RELKIND_RELATION:
		case RELKIND_TOASTVALUE:
		case RELKIND_MATVIEW:
			options = heap_reloptions(classForm->relkind, datum, false);
			break;
		case RELKIND_PARTITIONED_TABLE:
			options = partitioned_table_reloptions(datum, false);
			break;
		case RELKIND_VIEW:
			options = view_reloptions(datum, false);
			break;
		case RELKIND_INDEX:
		case RELKIND_PARTITIONED_INDEX:
			options = index_reloptions(amoptions, datum, false);
			break;
		case RELKIND_FOREIGN_TABLE:
			options = NULL;
			break;
		default:
			Assert(false);		/* can't get here */
			options = NULL;		/* keep compiler quiet */
			break;
	}


```

#2.heap_reloptions

```
heap_reloptions
--default_reloptions(reloptions, validate, RELOPT_KIND_HEAP);
----build_reloptions(reloptions, validate, kind,
									  sizeof(StdRdOptions),
									  tab, lengthof(tab));
```

#3.build_reloptions

```
build_reloptions
---options = parseRelOptions(reloptions, validate, kind, &numoptions);
--rdopts = allocateReloptStruct(relopt_struct_size, options, numoptions);
--fillRelOptions(rdopts, relopt_struct_size, options, numoptions,
				   validate, relopt_elems, num_relopt_elems);

```