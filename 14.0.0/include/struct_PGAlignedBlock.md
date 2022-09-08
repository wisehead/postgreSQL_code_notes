#1.struct PGAlignedBlock

```cpp
/*
 * Use this, not "char buf[BLCKSZ]", to declare a field or local variable
 * holding a page buffer, if that page might be accessed as a page and not
 * just a string of bytes.  Otherwise the variable might be under-aligned,
 * causing problems on alignment-picky hardware.  (In some places, we use
 * this to declare buffers even though we only pass them to read() and
 * write(), because copying to/from aligned buffers is usually faster than
 * using unaligned buffers.)  We include both "double" and "int64" in the
 * union to ensure that the compiler knows the value must be MAXALIGN'ed
 * (cf. configure's computation of MAXIMUM_ALIGNOF).
 */
typedef union PGAlignedBlock
{
	char		data[BLCKSZ];
	double		force_align_d;
	int64		force_align_i64;
} PGAlignedBlock;
```