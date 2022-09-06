#1.plainto_tsquery

```cpp

Datum
plainto_tsquery(PG_FUNCTION_ARGS)
{
    text       *in = PG_GETARG_TEXT_P(0);
    Oid         cfgId;

    cfgId = getTSCurrentConfig(true);
    PG_RETURN_DATUM(DirectFunctionCall2(plainto_tsquery_byid,
                                        ObjectIdGetDatum(cfgId),
                                        PointerGetDatum(in)));
}
```