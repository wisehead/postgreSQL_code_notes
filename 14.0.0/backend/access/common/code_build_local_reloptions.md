#1.build_local_reloptions

```
build_local_reloptions
--foreach(lc, relopts->options)
----local_relopt *opt = lfirst(lc);                                    
----elems[i].optname = opt->option->name;  
----elems[i].opttype = opt->option->type;  
----elems[i].offset = opt->offset;         
--vals = parseLocalRelOptions(relopts, options, validate);
----
```