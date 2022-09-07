#1.struct FormData_pg_amop

```cpp
/* ----------------
 *		pg_amop definition.  cpp turns this into
 *		typedef struct FormData_pg_amop
 * ----------------
 */
CATALOG(pg_amop,2602,AccessMethodOperatorRelationId)
{
	Oid			oid;			/* oid */

	/* the index opfamily this entry is for */
	Oid			amopfamily BKI_LOOKUP(pg_opfamily);

	/* operator's left input data type */
	Oid			amoplefttype BKI_LOOKUP(pg_type);

	/* operator's right input data type */
	Oid			amoprighttype BKI_LOOKUP(pg_type);

	/* operator strategy number */
	int16		amopstrategy;

	/* is operator for 's'earch or 'o'rdering? */
	char		amoppurpose BKI_DEFAULT(s);

	/* the operator's pg_operator OID */
	Oid			amopopr BKI_LOOKUP(pg_operator);

	/* the index access method this entry is for */
	Oid			amopmethod BKI_LOOKUP(pg_am);

	/* ordering opfamily OID, or 0 if search op */
	Oid			amopsortfamily BKI_DEFAULT(0) BKI_LOOKUP_OPT(pg_opfamily);
} FormData_pg_amop;

```