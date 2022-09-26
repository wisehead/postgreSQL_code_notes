#1.struct GenericXLogState

```cpp
/* State of generic xlog record construction */
struct GenericXLogState
{
	/* Info about each page, see above */
	PageData	pages[MAX_GENERIC_XLOG_PAGES];
	bool		isLogged;
	/* Page images (properly aligned) */
	PGAlignedBlock images[MAX_GENERIC_XLOG_PAGES];
};
```