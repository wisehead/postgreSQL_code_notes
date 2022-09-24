#1.struct FormData_pg_am

```cpp
/* ----------------
 *		pg_am definition.  cpp turns this into
 *		typedef struct FormData_pg_am
 * ----------------
 */
CATALOG(pg_am,2601,AccessMethodRelationId)
{
	Oid			oid;			/* oid */

	/* access method name */
	NameData	amname;

	/* handler function */
	regproc		amhandler BKI_LOOKUP(pg_proc);

	/* see AMTYPE_xxx constants below */
	char		amtype;
} FormData_pg_am;
```