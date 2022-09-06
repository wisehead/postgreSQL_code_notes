#1.palloc0

```cpp
void *
palloc0(Size size)
{
    return pg_malloc_internal(size, MCXT_ALLOC_ZERO);
}
```