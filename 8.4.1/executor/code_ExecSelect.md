#1.ExecSelect

```cpp
/* ----------------------------------------------------------------
 *      ExecSelect
 *
 *      SELECTs are easy.. we just pass the tuple to the appropriate
 *      output function.
 * ----------------------------------------------------------------
 */
static void
ExecSelect(TupleTableSlot *slot,
           DestReceiver *dest,
           EState *estate)

```