#1.CreateListPages

```
CreateListPages
--buf = IvfflatNewBuffer(index, forkNum);
--IvfflatInitRegisterPage
--for (i = 0; i < lists; i++)
		/* Load list */
----list->startPage = InvalidBlockNumber;
----list->insertPage = InvalidBlockNumber;
----memcpy(&list->center, VectorArrayGet(centers, i), VECTOR_SIZE(dimensions));
--if (PageGetFreeSpace(page) < itemsz)
----IvfflatAppendPage(index, &buf, &page, &state, forkNum);
```