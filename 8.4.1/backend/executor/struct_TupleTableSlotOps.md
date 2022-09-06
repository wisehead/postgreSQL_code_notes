#1.struct TupleTableSlotOps

```cpp

/* routines for a TupleTableSlot implementation */
struct TupleTableSlotOps
{
    /* Minimum size of the slot */
    size_t      base_slot_size;

    /* Initialization. */
    void        (*init) (TupleTableSlot *slot);

    /* Destruction. */
    void        (*release) (TupleTableSlot *slot);

    /*
     * Clear the contents of the slot. Only the contents are expected to be
     * cleared and not the tuple descriptor. Typically an implementation of
     * this callback should free the memory allocated for the tuple contained
     * in the slot.
     */
    void        (*clear) (TupleTableSlot *slot);

    /*
     * Fill up first natts entries of tts_values and tts_isnull arrays with
     * values from the tuple contained in the slot. The function may be called
     * with natts more than the number of attributes available in the tuple,
     * in which case it should set tts_nvalid to the number of returned
     * columns.
     */
    void        (*getsomeattrs) (TupleTableSlot *slot, int natts);

    /*
     * Returns value of the given system attribute as a datum and sets isnull
     * to false, if it's not NULL. Throws an error if the slot type does not
     * support system attributes.
     */
    Datum       (*getsysattr) (TupleTableSlot *slot, int attnum, bool *isnull);

    /*
     * Make the contents of the slot solely depend on the slot, and not on
     * underlying resources (like another memory context, buffers, etc).
     */
    void        (*materialize) (TupleTableSlot *slot);

    /*
     * Copy the contents of the source slot into the destination slot's own
     * context. Invoked using callback of the destination slot.
     */
    void        (*copyslot) (TupleTableSlot *dstslot, TupleTableSlot *srcslot);

    /*
     * Return a heap tuple "owned" by the slot. It is slot's responsibility to
     * free the memory consumed by the heap tuple. If the slot can not "own" a
     * heap tuple, it should not implement this callback and should set it as
     * NULL.
     */
    HeapTuple   (*get_heap_tuple) (TupleTableSlot *slot);

    /*
     * Return a minimal tuple "owned" by the slot. It is slot's responsibility
     * to free the memory consumed by the minimal tuple. If the slot can not
     * "own" a minimal tuple, it should not implement this callback and should
     * set it as NULL.
     */
    MinimalTuple (*get_minimal_tuple) (TupleTableSlot *slot);

    /*
     * Return a copy of heap tuple representing the contents of the slot. The
     * copy needs to be palloc'd in the current memory context. The slot
     * itself is expected to remain unaffected. It is *not* expected to have
     * meaningful "system columns" in the copy. The copy is not be "owned" by
     * the slot i.e. the caller has to take responsibility to free memory
     * consumed by the slot.
     */
    HeapTuple   (*copy_heap_tuple) (TupleTableSlot *slot);

    /*
     * Return a copy of minimal tuple representing the contents of the slot.
     * The copy needs to be palloc'd in the current memory context. The slot
     * itself is expected to remain unaffected. It is *not* expected to have
     * meaningful "system columns" in the copy. The copy is not be "owned" by
     * the slot i.e. the caller has to take responsibility to free memory
     * consumed by the slot.
     */
    MinimalTuple (*copy_minimal_tuple) (TupleTableSlot *slot);
};    
```