#1.QuickCenters

```cpp
QuickCenters
--normprocinfo = IvfflatOptionalProcInfo(index, IVFFLAT_KMEANS_NORM_PROC);
----index_getprocinfo(rel, 1, procnum);
--if (samples->length > 0)
----qsort(samples->items, samples->length, VECTOR_SIZE(samples->dim), CompareVectors);
----vec = VectorArrayGet(samples, i);
----if (i == 0 || CompareVectors(vec, VectorArrayGet(samples, i - 1)) != 0)
------VectorArraySet(centers, centers->length, vec);
------centers->length++;
--while (centers->length < centers->maxlen)
----vec = VectorArrayGet(centers, centers->length);             
----SET_VARSIZE(vec, VECTOR_SIZE(dimensions));                  
----vec->dim = dimensions;                                      
----for (j = 0; j < dimensions; j++)                            
------vec->x[j] = ((double) random()) / MAX_RANDOM_VALUE;       
----/* Normalize if needed (only needed for random centers) */  
----if (normprocinfo != NULL)                                   
------ApplyNorm(normprocinfo, collation, vec);                  
----centers->length++;                                          

```