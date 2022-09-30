#1.enum TypeFuncClass

```cpp
/* Type categories for get_call_result_type and siblings */
typedef enum TypeFuncClass
{
	TYPEFUNC_SCALAR,			/* scalar result type */
	TYPEFUNC_COMPOSITE,			/* determinable rowtype result */
	TYPEFUNC_COMPOSITE_DOMAIN,	/* domain over determinable rowtype result */
	TYPEFUNC_RECORD,			/* indeterminate rowtype result */
	TYPEFUNC_OTHER				/* bogus type, eg pseudotype */
} TypeFuncClass;
```