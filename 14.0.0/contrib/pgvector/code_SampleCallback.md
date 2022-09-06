#1.SampleCallback

```cpp
SampleCallback
--IvfflatNormValue
----FunctionCall1Coll
------InitFunctionCallInfoData
------FunctionCallInvoke(fcinfo);
--------((* (fcinfo)->flinfo->fn_addr) (fcinfo))
--------for (i = 0; i < v->dim; i++)
----------result->x[i] = v->x[i] / norm;
--if (samples->length < targsamples)
----VectorArraySet(samples, samples->length, DatumGetVector(value));
------(memcpy(VECTOR_ARRAY_OFFSET(_arr, _offset), _val, VECTOR_SIZE(_arr->dim)))
--//else
----if (buildstate->rowstoskip < 0)
------reservoir_get_next_S(&buildstate->rstate, samples->length, targsamples);
----if (buildstate->rowstoskip <= 0)
------k = (int) (targsamples * sampler_random_fract(buildstate->rstate.randstate));
------VectorArraySet(samples, k, DatumGetVector(value));

```