#1.palloc

```cpp
/*
 * Frontend emulation of backend memory management functions.  Useful for
 * programs that compile backend files.
 */
void *
palloc(Size size)
{
    return pg_malloc_internal(size, 0);
}
```