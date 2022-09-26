#1.GenericXLogFinish

```
GenericXLogFinish
--if (state->isLogged)
----XLogBeginInsert
----for (i = 0; i < MAX_GENERIC_XLOG_PAGES; i++)
------page = BufferGetPage(pageData->buffer);
------pageHeader = (PageHeader) pageData->image;
------if (pageData->flags & GENERIC_XLOG_FULL_IMAGE)
--------memcpy(page, pageData->image, pageHeader->pd_lower);
--------memset(page + pageHeader->pd_lower, 0,
					   pageHeader->pd_upper - pageHeader->pd_lower);
--------memcpy(page + pageHeader->pd_upper,
					   pageData->image + pageHeader->pd_upper,
					   BLCKSZ - pageHeader->pd_upper);
--------XLogRegisterBuffer
------else
--------computeDelta(pageData, page, (Page) pageData->image);
```