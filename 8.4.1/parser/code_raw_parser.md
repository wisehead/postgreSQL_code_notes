#1.raw_parser

```cpp

/*
 * raw_parser
 *      Given a query in string form, do lexical and grammatical analysis.
 *
 * Returns a list of raw (un-analyzed) parse trees.
 */
List *
raw_parser(const char *str)
```

#2.caller

```
PostgresMain
--exec_simple_query
----pg_parse_query
------raw_parser
```