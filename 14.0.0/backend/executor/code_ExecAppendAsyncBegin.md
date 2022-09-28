#1.ExecAppendAsyncBegin

```
ExecAppendAsyncBegin
--if (node->as_valid_subplans == NULL)
----node->as_valid_subplans =
			ExecFindMatchingSubPlans(node->as_prune_state);
----classify_matching_subplans(node);
--while ((i = bms_next_member(node->as_valid_asyncplans, i)) >= 0)
----AsyncRequest *areq = node->as_asyncrequests[i];
----ExecAsyncRequest(areq);
```

#2.ExecAsyncRequest

```
ExecAsyncRequest
--if (areq->requestee->chgParam != NULL)
----ExecReScan(areq->requestee);	/* let ReScan handle this */
--switch (nodeTag(areq->requestee))
----case T_ForeignScanState:
------ExecAsyncForeignScanRequest(areq);
--------fdwroutine->ForeignAsyncRequest(areq);
------break;
----default:
------break;
--ExecAsyncResponse(areq);

```