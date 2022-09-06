#1.struct FunctionCallInfoBaseData

```cpp
/*
 * This struct is the data actually passed to an fmgr-called function.
 *
 * The called function is expected to set isnull, and possibly resultinfo or
 * fields in whatever resultinfo points to.  It should not change any other
 * fields.  (In particular, scribbling on the argument arrays is a bad idea,
 * since some callers assume they can re-call with the same arguments.)
 *
 * Note that enough space for arguments needs to be provided, either by using
 * SizeForFunctionCallInfo() in dynamic allocations, or by using
 * LOCAL_FCINFO() for on-stack allocations.
 *
 * This struct is named *BaseData, rather than *Data, to break pre v12 code
 * that allocated FunctionCallInfoData itself, as it'd often silently break
 * old code due to no space for arguments being provided.
 */
typedef struct FunctionCallInfoBaseData
{
	FmgrInfo   *flinfo;			/* ptr to lookup info used for this call */
	fmNodePtr	context;		/* pass info about context of call */
	fmNodePtr	resultinfo;		/* pass or return extra info about result */
	Oid			fncollation;	/* collation for function to use */
#define FIELDNO_FUNCTIONCALLINFODATA_ISNULL 4
	bool		isnull;			/* function must set true if result is NULL */
	short		nargs;			/* # arguments actually passed */
#define FIELDNO_FUNCTIONCALLINFODATA_ARGS 6
	NullableDatum args[FLEXIBLE_ARRAY_MEMBER];
} FunctionCallInfoBaseData;
```