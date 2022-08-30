#1.gistbuild

```cpp
/*
 * Routine to build an index.  Basically calls insert over and over.
 *
 * XXX: it would be nice to implement some sort of bulk-loading
 * algorithm, but it is not clear how to do that.
 */
Datum
gistbuild(PG_FUNCTION_ARGS)

```