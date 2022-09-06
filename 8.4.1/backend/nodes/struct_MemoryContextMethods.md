#1.struct MemoryContextMethods

```cpp
/*
 * MemoryContext
 *      A logical context in which memory allocations occur.
 *
 * MemoryContext itself is an abstract type that can have multiple
 * implementations, though for now we have only AllocSetContext.
 * The function pointers in MemoryContextMethods define one specific
 * implementation of MemoryContext --- they are a virtual function table
 * in C++ terms.
 *
 * Node types that are actual implementations of memory contexts must
 * begin with the same fields as MemoryContext.
 *
 * Note: for largely historical reasons, typedef MemoryContext is a pointer
 * to the context struct rather than the struct type itself.
 */

typedef struct MemoryContextMethods
{
    void       *(*alloc) (MemoryContext context, Size size);
    /* call this free_p in case someone #define's free() */
    void        (*free_p) (MemoryContext context, void *pointer);
    void       *(*realloc) (MemoryContext context, void *pointer, Size size);
    void        (*init) (MemoryContext context);
    void        (*reset) (MemoryContext context);
    void        (*delete) (MemoryContext context);
    Size        (*get_chunk_space) (MemoryContext context, void *pointer);
    bool        (*is_empty) (MemoryContext context);
    void        (*stats) (MemoryContext context, int level);
#ifdef MEMORY_CONTEXT_CHECKING
    void        (*check) (MemoryContext context);
#endif
} MemoryContextMethods;

```