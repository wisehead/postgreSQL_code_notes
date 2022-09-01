#1.ExecutePlan


```cpp
/* ----------------------------------------------------------------
 *      ExecutePlan
 *
 *      Processes the query plan until we have processed 'numberTuples' tuples,
 *      moving in the specified direction.
 *
 *      Runs to completion if numberTuples is 0
 *
 * Note: the ctid attribute is a 'junk' attribute that is removed before the
 * user can see it
 * ----------------------------------------------------------------
 */
static void
ExecutePlan(EState *estate,
            PlanState *planstate,
            CmdType operation,
            long numberTuples,
            ScanDirection direction,
            DestReceiver *dest)

```

#2.caller

- standard_ExecutorRun
- 