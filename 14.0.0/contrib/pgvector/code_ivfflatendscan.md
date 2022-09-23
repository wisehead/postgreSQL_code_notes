#1.ivfflatendscan

```
ivfflatendscan
--if (BufferIsValid(so->buf))
----ReleaseBuffer(so->buf);
--pairingheap_free(so->listQueue);
--tuplesort_end(so->sortstate);
----tuplesort_free(state);
```