#1.LocalBufferAlloc

```
LocalBufferAlloc
--if (LocalBufHash == NULL)
----InitLocalBuffers();
--hresult = (LocalBufferLookupEnt *)
		hash_search(LocalBufHash, (void *) &newTag, HASH_FIND, NULL);
--if (hresult)
----b = hresult->id;
----bufHdr = GetLocalBufferDescriptor(b);
------LocalBufferDescriptors[b]
----buf_state = pg_atomic_read_u32(&bufHdr->state);
----if (LocalRefCount[b] == 0)
------if (BUF_STATE_GET_USAGECOUNT(buf_state) < BM_MAX_USAGE_COUNT)
--------buf_state += BUF_USAGECOUNT_ONE;
--------pg_atomic_unlocked_write_u32(&bufHdr->state, buf_state);
----LocalRefCount[b]++;
----return bufHdr;
--for (;;)
----b = nextFreeLocalBuf;
----if (++nextFreeLocalBuf >= NLocBuffer)
------nextFreeLocalBuf = 0;
----bufHdr = GetLocalBufferDescriptor(b);
----if (LocalRefCount[b] == 0)
------buf_state = pg_atomic_read_u32(&bufHdr->state);
------if (BUF_STATE_GET_USAGECOUNT(buf_state) > 0)
--------buf_state -= BUF_USAGECOUNT_ONE;
--------pg_atomic_unlocked_write_u32(&bufHdr->state, buf_state);
--------trycounter = NLocBuffer;
------else
--------LocalRefCount[b]++;
--------break;
----else if (--trycounter == 0)
------ereport
--if (buf_state & BM_DIRTY)
----localpage = (char *) LocalBufHdrGetBlock(bufHdr);
------LocalBufferBlockPointers[-((bufHdr)->buf_id + 2)]
----smgropen(bufHdr->tag.rnode, MyBackendId);
----PageSetChecksumInplace
----smgrwrite(oreln,
				  bufHdr->tag.forkNum,
				  bufHdr->tag.blockNum,
				  localpage,
				  false);
------mdwrite
----buf_state &= ~BM_DIRTY;
----pg_atomic_unlocked_write_u32(&bufHdr->state, buf_state);
--if (LocalBufHdrGetBlock(bufHdr) == NULL)
----LocalBufHdrGetBlock(bufHdr) = GetLocalBufferStorage();
```