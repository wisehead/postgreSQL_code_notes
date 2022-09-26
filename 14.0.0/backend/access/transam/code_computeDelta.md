#1.computeDelta

```
computeDelta
--	/* Compute delta records for lower part of page ... */
--computeRegionDelta(pageData, curpage, targetpage,
					   0, targetLower,
					   0, curLower);
	/* ... and for upper part, ignoring what's between */
--computeRegionDelta(pageData, curpage, targetpage,
					   targetUpper, BLCKSZ,
					   curUpper, BLCKSZ);
```

#2.computeRegionDelta

```
computeRegionDelta
--writeFragment
```