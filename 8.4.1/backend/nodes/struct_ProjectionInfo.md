#1.struct ProjectionInfo

```cpp
/* ----------------
 *      ProjectionInfo node information
 *
 *      This is all the information needed to perform projections ---
 *      that is, form new tuples by evaluation of targetlist expressions.
 *      Nodes which need to do projections create one of these.
 *
 *      ExecProject() evaluates the tlist, forms a tuple, and stores it
 *      in the given slot.  Note that the result will be a "virtual" tuple
 *      unless ExecMaterializeSlot() is then called to force it to be
 *      converted to a physical tuple.  The slot must have a tupledesc
 *      that matches the output of the tlist!
 *
 *      The planner very often produces tlists that consist entirely of
 *      simple Var references (lower levels of a plan tree almost always
 *      look like that).  And top-level tlists are often mostly Vars too.
 *      We therefore optimize execution of simple-Var tlist entries.
 *      The pi_targetlist list actually contains only the tlist entries that
 *      aren't simple Vars, while those that are Vars are processed using the
 *      varSlotOffsets/varNumbers/varOutputCols arrays.
 *
 *      The lastXXXVar fields are used to optimize fetching of fields from
 *      input tuples: they let us do a slot_getsomeattrs() call to ensure
 *      that all needed attributes are extracted in one pass.
 *
 *      targetlist      target list for projection (non-Var expressions only)
 *      exprContext     expression context in which to evaluate targetlist
 *      slot            slot to place projection result in
 *      itemIsDone      workspace array for ExecProject
 *      directMap       true if varOutputCols[] is an identity map
 *      numSimpleVars   number of simple Vars found in original tlist
 *      varSlotOffsets  array indicating which slot each simple Var is from
 *      varNumbers      array containing input attr numbers of simple Vars
 *      varOutputCols   array containing output attr numbers of simple Vars
 *      lastInnerVar    highest attnum from inner tuple slot (0 if none)
 *      lastOuterVar    highest attnum from outer tuple slot (0 if none)
 *      lastScanVar     highest attnum from scan tuple slot (0 if none)
 * ----------------
 */
typedef struct ProjectionInfo
{
    NodeTag     type;
    List       *pi_targetlist;
    ExprContext *pi_exprContext;
    TupleTableSlot *pi_slot;
    ExprDoneCond *pi_itemIsDone;
    bool        pi_directMap;
    int         pi_numSimpleVars;
    int        *pi_varSlotOffsets;
    int        *pi_varNumbers;
    int        *pi_varOutputCols;
    int         pi_lastInnerVar;
    int         pi_lastOuterVar;
    int         pi_lastScanVar;
} ProjectionInfo;
```