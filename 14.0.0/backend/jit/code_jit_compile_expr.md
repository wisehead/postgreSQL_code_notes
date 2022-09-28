#1.jit_compile_expr

```
jit_compile_expr
--if (provider_init())
----return provider.compile_expr(state);
```

#2. provider_init

```
provider_init
--init = (JitProviderInit)
----load_external_function(path, "_PG_jit_provider_init", true, NULL);
--init(&provider);

```