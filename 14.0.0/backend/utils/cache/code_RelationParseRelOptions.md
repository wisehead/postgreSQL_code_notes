#1.RelationParseRelOptions

```
RelationParseRelOptions
--switch (relation->rd_rel->relkind)
	{
		case RELKIND_RELATION:
		case RELKIND_TOASTVALUE:
		case RELKIND_VIEW:
		case RELKIND_MATVIEW:
		case RELKIND_PARTITIONED_TABLE:
			amoptsfn = NULL;
			break;
		case RELKIND_INDEX:
		case RELKIND_PARTITIONED_INDEX:
			amoptsfn = relation->rd_indam->amoptions;
			break;
		default:
			return;
	}
--options = extractRelOptions(tuple, GetPgClassDescriptor(), amoptsfn);
```