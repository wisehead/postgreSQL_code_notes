#1.build_local_reloptions

```
build_local_reloptions
--foreach(lc, relopts->options)
----local_relopt *opt = lfirst(lc);                                    
----elems[i].optname = opt->option->name;  
----elems[i].opttype = opt->option->type;  
----elems[i].offset = opt->offset;         
--vals = parseLocalRelOptions(relopts, options, validate);
--opts = allocateReloptStruct(relopts->relopt_struct_size, vals, noptions);
--fillRelOptions(opts, relopts->relopt_struct_size, vals, noptions, 		validate,elems, noptions);
--foreach(lc, relopts->validators)
		((relopts_validator) lfirst(lc)) (opts, vals, noptions);
```

#2. parseLocalRelOptions
```
parseLocalRelOptions
--foreach(lc, relopts->options)
----values[i].gen = opt->option;
--parseRelOptionsInternal(options, validate, values, nopts);
```

#3.parseRelOptionsInternal

```
parseRelOptionsInternal
--ArrayType  *array = DatumGetArrayTypeP(options);
--deconstruct_array(array, TEXTOID, -1, false, TYPALIGN_INT,
					  &optiondatums, NULL, &noptions);
--for (i = 0; i < noptions; i++)
----char	   *text_str = VARDATA(optiondatums[i]);
----int			text_len = VARSIZE(optiondatums[i]) - VARHDRSZ;
----for (j = 0; j < numoptions; j++)
------parse_one_reloption(&reloptions[j], text_str, text_len,validate);
------break;
```

#4.parse_one_reloption

```
parse_one_reloption
--switch (option->gen->type)
----case RELOPT_TYPE_BOOL:
------parsed = parse_bool(value, &option->values.bool_val);
----case RELOPT_TYPE_INT:
------parsed = parse_int(value, &option->values.int_val, 0, NULL);
```

#5. fillRelOptions//copy

```
fillRelOptions
--for (i = 0; i < numoptions; i++)
----for (j = 0; j < numelems; j++)
------if (strcmp(options[i].gen->name, elems[j].optname) == 0)
```











