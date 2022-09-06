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

```