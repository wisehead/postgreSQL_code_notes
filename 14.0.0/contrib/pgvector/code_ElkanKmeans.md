#1.ElkanKmeans

```
ElkanKmeans
--procinfo = index_getprocinfo(index, 1, IVFFLAT_KMEANS_DISTANCE_PROC);
--normprocinfo = IvfflatOptionalProcInfo(index, IVFFLAT_KMEANS_NORM_PROC);
--newCenters = VectorArrayInit(numCenters, dimensions);
--for (j = 0; j < numCenters; j++)
----vec = VectorArrayGet(newCenters, j);
----SET_VARSIZE(vec, VECTOR_SIZE(dimensions));
----vec->dim = dimensions;
--InitCenters(index, samples, centers, lowerBound);
```

#2.InitCenters

```
InitCenters
--procinfo = index_getprocinfo(index, 1, IVFFLAT_KMEANS_DISTANCE_PROC);
--VectorArraySet(centers, 0, VectorArrayGet(samples, random() % samples->length));
--for (i = 0; i < numCenters; i++)
----for (j = 0; j < numSamples; j++)
------vec = VectorArrayGet(samples, j);
------distance = DatumGetFloat8(FunctionCall2Coll(procinfo, collation, PointerGetDatum(vec), PointerGetDatum(VectorArrayGet(centers, i))));
------lowerBound[j * numCenters + i] = distance;
----choice = sum * (((double) random()) / MAX_RANDOM_VALUE);
----for (j = 0; j < numSamples - 1; j++)
		{
			choice -= weight[j];
			if (choice <= 0)
				break;
		}
----VectorArraySet(centers, i + 1, VectorArrayGet(samples, j));
----centers->length++;
```