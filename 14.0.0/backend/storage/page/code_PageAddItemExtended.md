#1.PageAddItemExtended

```
PageAddItemExtended
--limit = OffsetNumberNext(PageGetMaxOffsetNumber(page));
--if (OffsetNumberIsValid(offsetNumber))
----if ((flags & PAI_OVERWRITE) != 0)
------if (offsetNumber < limit)
--------itemId = PageGetItemId(phdr, offsetNumber);
--------if (ItemIdIsUsed(itemId) || ItemIdHasStorage(itemId))
----------return InvalidOffsetNumber;
--else
----if (PageHasFreeLinePointers(phdr))
------for (offsetNumber = FirstOffsetNumber;
				 offsetNumber < limit;	/* limit is maxoff+1 */
				 offsetNumber++)
--------itemId = PageGetItemId(phdr, offsetNumber);
--------if (!ItemIdIsUsed(itemId) && !ItemIdHasStorage(itemId))
					break;
------if (offsetNumber >= limit)
--------PageClearHasFreeLinePointers(phdr);
----else
------offsetNumber = limit;
--if (needshuffle)
		memmove(itemId + 1, itemId,
				(limit - offsetNumber) * sizeof(ItemIdData));
--ItemIdSetNormal(itemId, upper, size);
--memcpy((char *) page + upper, item, size);
```
